---
name: comment-cleanup
description: Sift through source code and clean up comments that were written for an AI assistant's benefit rather than for human maintainers - reasoning trails, self-directed notes, justifications addressed to a reviewer, and comments that just narrate what the next line does. Replace what's worth keeping with concise, conventional documentation (docstrings/JSDoc/doc-comments) and delete the rest. Use this whenever the user asks to clean up, declutter, or "de-AI-ify" comments in a file or directory, mentions comments that are "bloated," "over-explained," feel like they're talking to an AI, or asks for a pass to make code comments more human-readable/professional before committing or shipping.
---

# Comment Cleanup

Source code that's been heavily worked on by an AI assistant often accumulates comments that made sense as reasoning-in-progress but don't belong in the shipped file. They explain *why the AI chose something* to a reviewer, restate what adjacent code already makes obvious, or narrate the AI's own thought process. This skill finds and fixes that pattern: strip what isn't useful to a human reading the code later, and upgrade what genuinely deserves documentation into a concise, idiomatic doc comment.

## Why this matters

A human maintaining this code later doesn't care why an AI picked one data structure over another mid-conversation, or that a variable is "same idiom as the old one, just bigger." They care about the current contract: what does this function guarantee, what are the preconditions, what will surprise them if they change it. Comments that read like a chat transcript bloat the file and bury the comments that actually matter under paragraphs of throat-clearing. The goal isn't zero comments - it's comments that pull their weight for a human six months from now who has none of this conversation's context.

## Two categories of comment to fix

**1. AI-directed notes.** Comments addressed to an assistant or reviewer rather than to a future maintainer. Signals: talks about "the old version" or "what I changed," references a conversation or a spec doc as if the reader was there for it, explains a choice in terms of a prior state of the code rather than the current contract, uses first person about a decision ("kept this because..."), or is phrased as self-justification rather than as information about the code. These almost always should be deleted outright - the reasoning was useful during the edit, not after it.

**2. Over-explaining comments.** Comments that narrate what the code already says, restate the obvious, or bury a genuinely useful fact (a precondition, a gotcha, an invariant) inside several sentences of padding. These should be reduced to the useful fact only, and if that fact belongs on a function/type/class rather than a random line, promoted to a proper doc comment instead of an inline one.

Example of both problems in one place (from a C++ header):

```cpp
// One layer's room grid - a plain 100x100 C array, same idiom the old 50x50 `map` global used
// (kept as a typedef'd C array rather than std::array so `(&grid)[100][100]`-style reference
// parameters keep working exactly like `(&rooms)[50][50]` did, just bigger).
using RoomGrid = _roomType[100][100];

struct WorldState
{
	std::vector<std::unique_ptr<RoomGrid>> layers; // index = layer number

	// Returns nullptr if the layer isn't loaded, or row/col is out of [0,100) range - callers must
	// null-check. Never throws, never indexes out of bounds.
	_roomType* getRoom(int layer, int row, int col) { ... }
};
```

The `RoomGrid` comment is entirely AI-directed - it justifies a design decision by referencing "the old version," which means nothing to someone who never saw the old version. Delete it. The `getRoom` comment has one genuinely useful sentence (the nullptr/bounds contract) buried under justification - keep the contract, cut the rest, and turn it into a proper doc comment. Note the doc comment lives at the struct level, not on the individual method (see "Where doc comments go" below):

```cpp
using RoomGrid = _roomType[100][100];

/// getRoom: returns nullptr if the layer isn't loaded or (row, col) is out of [0,100) range.
struct WorldState
{
	std::vector<std::unique_ptr<RoomGrid>> layers; // index = layer number

	_roomType* getRoom(int layer, int row, int col) { ... }
};
```

## Where doc comments go

Doc comments belong at the top-level declaration for the surrounding unit - the struct, class, module, or file - not scattered above each individual method or field. When several methods in the same struct/class each have a worthwhile comment, combine them into **one block above the top-level declaration**, with each method's contribution labeled by name so it's still scannable:

```cpp
/// A sparse collection of world layers, one per loaded main*.xml file. layers[i] is null
/// for any layer index with no file - not every index 0-99 is populated.
///
/// getRoom: returns nullptr if the layer isn't loaded, or row/col is out of [0,100) range.
/// Never throws, never indexes out of bounds.
///
/// ensureLayer: ensures layers[layer] exists, allocating a fresh grid if needed, and
/// returns a reference to it.
struct WorldState { ... };
```

This keeps the actual method/field bodies free of comment clutter and gives a reader one place to look for "what does this whole unit do and guarantee" rather than hunting line by line. The only comments that stay inline at their original spot are terse ones (a few words, like `// index = layer number`) that annotate a single line and wouldn't gain anything from being pulled up and labeled.

**When there's no natural top-level unit to consolidate into**, don't force one. In a `.cpp`-style implementation file where a function is defined outside any class body (the class declaration lives elsewhere, e.g. in the header), there's nothing above it to hoist the comment onto - keep the doc comment directly above that individual function instead. The "combine into one block" rule applies when you're inside a struct/class/module body with multiple documented members; it doesn't mean inventing an artificial anchor point when the file's structure doesn't have one.

## What to leave alone

Not every comment is a target. Leave in place:
- Comments explaining *why* the code does something non-obvious for reasons intrinsic to the domain (a workaround for a library bug, a performance tradeoff, a business rule) - these are exactly what good comments are for, as long as they're phrased for a maintainer and not for a reviewer of a diff.
- `TODO`/`FIXME` markers that describe real outstanding work, not AI self-notes.
- License headers, copyright notices, autogenerated-file banners.
- Comments in code you weren't asked to touch. Stay scoped to the files/directory the user pointed you at.
- Commented-out dead code (disabled debug lines, old implementations left in comments, etc.). This skill only deals with documentation comments - whether to remove dead code is a separate decision the user hasn't asked you to make here. If you notice some, you can mention it in the final summary, but don't touch it.

When genuinely unsure whether a comment is worth keeping, use your best judgment and err toward keeping domain knowledge and cutting narration/justification. Don't ask the user mid-pass - note anything you were on the fence about in the summary at the end instead.

## Process

1. **Scope the pass.** Confirm (or infer from the request) which files or directories to sift through. Default to the current directory if the user says something like "the current directory" or doesn't specify further and context makes it clear.

2. **Read before editing.** Skim each target file fully first. Getting a sense of the whole file's comment density and style prevents a piecemeal pass that misses patterns (e.g. every function in a file has the same AI-narration habit).

3. **Detect language by file extension** and follow that language's normal doc-comment convention when promoting a comment to documentation:
   - C/C++: `///` or `/** */` above the declaration
   - Python: docstring (`"""..."""`) as the first statement in the class/module body
   - JS/TS: JSDoc (`/** */`) above the declaration
   - Go: comment directly above the declaration starting with the identifier name, per Go convention
   - Java/C#: `/** */` Javadoc/XMLdoc style
   - Rust: `///` doc comments
   - Others: use whatever that language's ecosystem treats as the standard doc-comment form; fall back to a concise plain comment directly above the declaration if the language has no strong convention

   In all languages, place the resulting doc comment at the top-level unit (struct/class/module/file) per "Where doc comments go" above, not on each individual member - even if the language's usual convention is to document methods individually. For Python specifically, this means a class-level docstring covering its methods rather than a docstring inside each method body.

4. **Edit in place.** Make the changes directly in the source files. Don't create `.cleaned` copies or ask for confirmation before applying - the user has asked for this pass to just happen. Preserve all actual code untouched; this is a comment-only pass. Don't refactor, rename, or reformat code while doing this unless a comment change requires touching the same line (e.g. converting an inline `//` above a function into a `///` doc comment block).

5. **Summarize when done.** After the pass, give the user a short summary: roughly how many comments were removed vs. rewritten into doc comments, and call out anything you left in on purpose because it seemed like a borderline call (so they can double check your judgment on the ones that matter most).

## Notes on judgment

- A comment can be *both* AI-directed and over-explaining at once (as in the example above) - handle it as one edit, not two passes.
- Don't over-trim. If cutting a comment down to "the useful fact" would leave something so terse it's cryptic, keep an extra clause rather than optimizing purely for brevity. The target is a maintainer's comment, not the shortest possible string.
- If a file has essentially no problematic comments, say so plainly rather than inventing changes to make - not every file needs editing.
- Large directories: process file by file rather than trying to hold the whole tree's context at once, and keep a running tally for the final summary.
