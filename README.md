# Ariadne

A thread through Obsidian's accessibility gaps.

Ariadne is a community project working on stopgap fixes for [Obsidian](https://obsidian.md)'s screen-reader and keyboard accessibility, plus a public catalog of known issues — what's broken, where it actually lives (Obsidian core, CodeMirror 6, Electron, or something a plugin can reach), and what's already being worked on.

## Why this exists

Obsidian has real, documented accessibility gaps for screen reader users — in the editor, in suggestion popups (like the `[[` wikilink autocomplete), in fold state, and in the settings dialog, among others. Some of these are fixable today via the community plugin API. Some live deeper — in Obsidian's own closed UI chrome, or even in Electron's accessibility bridge — and need upstream fixes that plugins can't provide.

Ariadne exists to do two things while those upstream fixes happen:

1. **Ship plugins** that patch the gaps we *can* reach through the documented plugin API (CodeMirror 6 extensions, `EditorSuggest`, DOM-level ARIA retrofits).
2. **Document clearly** which issues are plugin-fixable and which aren't, so effort — ours and the community's — goes where it can actually land, and so upstream bug reports have the detail needed to get prioritized.

This is a stopgap, not a substitute for Obsidian fixing these issues at the source. The goal is to make the gap smaller and more visible, not to pretend it isn't there.

## Status

Early. The catalog is seeded from initial research; the first plugin (an ARIA retrofit for the wikilink suggestion popup) hasn't been built yet.

## What's here

- [`docs/CATALOG.md`](docs/CATALOG.md) — the living list of known accessibility issues, which layer each one lives in, and its status.
- Individual plugins will each get their own repo, per Obsidian's community plugin conventions (a plugin needs `manifest.json`, `main.js`, and `versions.json` at its repo root to be installable). This repo is the hub — links to each plugin repo will land here as they exist.

## Related work

- [AuraGraph](https://community.obsidian.md/plugins/auragraph) by Bora FIRLANGEÇ — an accessible, keyboard-first rebuild of Obsidian's Graph View (ARIA architecture, audio feedback, screen reader support). Not part of this project, but doing real work in the same spirit — worth knowing about if you're exploring this space.

## Contributing

Not formally open yet, but if you're hitting Obsidian accessibility issues yourself: open an issue describing what you're seeing, which screen reader/platform, and reproduction steps. That's useful even before there's a fix — it's exactly the kind of detail the catalog needs.

## License

[MIT](LICENSE)
