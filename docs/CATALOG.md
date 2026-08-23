# Accessibility Issue Catalog

A living list of known Obsidian screen-reader/keyboard accessibility issues. The point of this catalog is to sort issues by **where they actually live**, since that determines who can fix them:

- **Plugin-fixable** — reachable through Obsidian's documented plugin API (CodeMirror 6 extensions via `registerEditorExtension`, the `EditorSuggest` class, DOM patching of Obsidian-owned UI). Ariadne can build this.
- **Needs upstream (Obsidian)** — lives in Obsidian's own closed UI chrome (e.g. the core Settings modal) with no plugin hook to reach it. Needs a bug report / feature request to the Obsidian team.
- **Needs upstream (CodeMirror)** — a genuine bug in CM6 itself, not in how Obsidian uses it. Rare — CM6 already has solid accessibility support in most areas (autocomplete, fold announcements). Needs a report to the CodeMirror project.
- **Needs upstream (Electron)** — below both Obsidian and CM6, in Electron/Chromium's accessibility tree bridge. Neither a plugin nor an upstream CM6/Obsidian fix reaches this.
- **Needs investigation** — not yet confirmed which layer it's in.
- **Fixed** — shipped in a plugin and confirmed working via live screen reader testing, not just "builds without error."

Confidence is noted per issue since some of this is still inference from testing/forum reports, not confirmed by reading Obsidian's source.

| # | Issue | Area | Layer | Confidence | Notes |
|---|-------|------|-------|------------|-------|
| 1 | `[[` wikilink and `#` tag autocomplete: no ARIA announcement that suggestions are available; no keyboard navigation semantics exposed to AT | Editor — suggestion popup | **Fixed** | High | Fixed in [ariadne-autocomplete](https://github.com/ssawczyn/ariadne-autocomplete). Confirmed live on macOS/VoiceOver for both wikilink and tag autocomplete, with no code changes needed between the two — confirms the fix is generic to Obsidian's shared suggestion-popup component. Obsidian's popup is a bespoke `EditorSuggest` UI, not built on CM6's `@codemirror/autocomplete`, which is why none of this worked natively. Not yet tested on Windows/NVDA or other AT+OS combinations — help wanted. |
| 2 | Folded headings/list items: fold state not conveyed programmatically (no `aria-expanded` equivalent) | Editor — Live Preview fold widgets | Plugin-fixable, likely | Medium | CM6's own fold system (`@codemirror/language` fold + `foldGutter`) already includes screen-reader announcements on fold/unfold. Obsidian's heading-fold arrows may be a separate custom widget that doesn't route through that. Needs confirmation of which fold implementation is actually in play before scoping a fix. |
| 3 | Settings dialog: core tabs and modal not fully accessible (labeling, focus management) | Settings UI | Needs upstream (Obsidian) | High | Plugins can only build their *own* settings tab, not touch the core Settings modal or other core tabs. A plugin-side fix here would mean unsupported monkey-patching of internal classes — fragile, breaks on updates. |
| 4 | Notes pane content becomes undetectable to screen readers after the Electron 30 upgrade (installer 1.7.0+) | App shell | Needs upstream (Electron), possibly also Obsidian | Medium | Reported on the Obsidian forum after the Electron 30 bump. Could be Electron's AT bridge, or how Obsidian mounts content within it — needs confirmation. |
| 5 | Reading/Preview mode: screen reader (NVDA) stops reading when it hits a link or embedded element | Reading view | Needs investigation | Low | Reported on forum; not yet traced to a specific layer. |
| 6 | Command palette and Quick Switcher selections not announced | Core UI | **Fixed** | High | Fixed in [ariadne-autocomplete](https://github.com/ssawczyn/ariadne-autocomplete). Root-caused by reading Obsidian's own installed app bundle (`obsidian.asar`) rather than guessing: command palette and Quick Switcher share one internal "Prompt" component (confirmed by a literal source comment: `/* Prompts - e.g. quick switcher, command palette */`). Its results container is `.prompt-results`, not `.suggestion-container` — which is why the original detection never fired against it. Individual items still use the same `.suggestion-item` / `.is-selected` mechanics as the inline editor popups, so once detection recognized `.prompt-results` as a valid container, everything downstream (tagging, activedescendant tracking, count announcements) applied unchanged. Confirmed live on macOS/VoiceOver for both command palette and Quick Switcher. Not yet tested on Windows/NVDA. |
| 7 | File explorer pane: no keyboard navigation | Core UI | Needs upstream (Obsidian), likely | Low | Not yet confirmed there's no plugin-reachable hook. |
| 8 | iOS/iPad: preview/edit toggle unreachable via VoiceOver | Mobile UI | Needs upstream (Obsidian) | Low | Mobile UI chrome, likely not plugin-reachable the same way desktop is. |
| 9 | Canvas: no keyboard navigation | Canvas feature | Needs upstream (Obsidian) | Low | [AuraGraph](https://community.obsidian.md/plugins/auragraph) took a plugin-side approach to a similar problem in Graph View — may be worth studying as a model, even though Canvas is a different feature. |

## How issues get added

Anyone hitting an accessibility problem in Obsidian can open an issue with: what you did, what you expected, what actually happened, your screen reader + OS + Obsidian version. It doesn't need a root-cause diagnosis — that's worked out here once there's enough detail to investigate.
