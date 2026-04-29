# Red Pen Tool Review

## What the tool does

Red Pen is a single-page browser app for reviewing AI-generated text. It:

1. Accepts pasted source text.
2. Renders a lightweight Markdown preview while retaining a character-level source map.
3. Lets users select text spans, add comments, and stores annotations with raw text offsets.
4. Draws highlight overlays in the rendered view using `Range.getClientRects()`.
5. Generates a structured “apply these edits” prompt for copy/paste into an LLM.
6. Autosaves sessions in `localStorage`.

## Issues found

### 1) Reverse text selection can fail to create annotations
Selection handling assumes `range.startContainer` appears before `range.endContainer` in the text-node walk. In some reverse-selection or complex DOM cases, `rendStart` may not resolve correctly and annotation creation silently fails.

**Impact:** Intermittent inability to annotate depending on selection direction.

### 2) Link renderer allows unsafe URL schemes
Markdown links are emitted as:

```html
<a href="..." target="_blank">
```

without validating the URL scheme and without `rel="noopener noreferrer"`.

**Impact:**
- `javascript:` or other unsafe schemes may be inserted by pasted content.
- `target="_blank"` without `rel` exposes `window.opener` risks.

### 3) Autosave indicator may display stale “saved” state when storage write fails
`saveSession` swallows `localStorage` exceptions and still leaves UI in a state that can imply successful autosave.

**Impact:** Confusing UX in private mode/quota/full storage scenarios.

### 4) Accessibility gaps in interactive overlay and controls
Overlay highlight rectangles are clickable `div`s only, and several interactive elements lack keyboard affordances and ARIA metadata.

**Impact:** Reduced keyboard/screen-reader usability.

### 5) Markdown parser is intentionally minimal but has edge-case mismatches
Inline parsing is regex/index based and can mis-handle nested/emphasis edge cases, escaped characters, and some malformed markdown combinations.

**Impact:** Rendered preview may diverge from user expectations for complex markdown.

## Improvements recommended

1. Normalize and robustly resolve selection boundaries using `Range.compareBoundaryPoints` or a DOM-position utility.
2. Sanitize links (`http:`, `https:`, optionally `mailto:`), and add `rel="noopener noreferrer"` on external links.
3. Distinguish save success vs failure in UI state; show non-blocking warning on failed persistence.
4. Improve accessibility:
   - keyboard activation for highlight interactions,
   - appropriate `role`, `aria-label`, and focus styles,
   - semantic button elements where practical.
5. Add tests (even lightweight browser tests) for:
   - selection mapping correctness,
   - markdown source-map integrity,
   - persistence fallback behavior,
   - prompt generation output stability.

## Nice-to-have enhancements

- Export format options (plain list, JSON, markdown checklist).
- Optional annotation categories/severity.
- Diff-oriented prompt mode (before/after snippets).
- Undo/redo stack for annotation actions.
