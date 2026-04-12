# ✏️ Red Pen

**Give Claude's output the red pen treatment.**

Red Pen is a lightweight, single-file browser tool for annotating AI-generated text. Paste any document, highlight the parts you want changed, leave editor's notes, and export a ready-to-send prompt back to Claude.

🔗 **Live at [redpen.dimitrios.im](https://redpen.dimitrios.im)**

---

## What it does

Most people copy Claude's output into a doc, rewrite bits manually, and iterate in their head. Red Pen makes that feedback loop explicit — you mark up the text like an editor would, then export the whole thing as a structured prompt.

1. **Paste** — drop in any Claude response or document (Markdown supported)
2. **Highlight** — select text, leave a note about what should change
3. **Export** — get a formatted prompt listing every edit, ready to copy back into Claude

---

## Features

- 📝 **Inline annotation** — highlight any text span, attach a note, repeat
- 🎨 **Color-coded highlights** — up to 5 colors, cycling automatically, with numbered badges
- 📋 **Sidebar panel** — all notes in one place, click to activate, edit inline, remove individually
- 📤 **Prompt export** — auto-generates a structured "please apply these edits" prompt; editable before copying
- 💾 **Autosave** — session persists in `localStorage` across page reloads; restore banner on return
- 🌙 **Light / dark mode** — toggleable from the header
- 🔤 **Markdown rendering** — headings, bold, italic, code, blockquotes, tables, lists all render correctly
- ⌨️ **Keyboard shortcuts** — `Cmd/Ctrl+Enter` to save a note, `Esc` to cancel

---

## How annotation works

Red Pen builds a **source map** during Markdown rendering — a character-level index that tracks which rendered character came from which position in the raw input. When you make a selection, it maps the DOM range back to the raw offsets, so annotations stay anchored to the actual source text regardless of how it's displayed.

Highlights are drawn on an absolute-positioned overlay using `Range.getClientRects()`, merged to remove duplicates on the same line.

---

## Stack

- Pure HTML + vanilla JS — no framework, no build step, no dependencies
- `localStorage` for session persistence
- IBM Plex Sans / IBM Plex Mono / Instrument Serif (Google Fonts)
- Deployed on GitHub Pages with a custom domain via `CNAME`

---

## Part of [approximate.work](https://approximate.work)

Red Pen is one of the tools in the approximate.work portfolio — small, focused utilities built around real workflows.
