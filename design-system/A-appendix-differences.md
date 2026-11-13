# Appendix: differences, known gaps, not verified

Read on 2026-11-13. Reference library: `Timebase.Core.React.UI` at commit `137b848b9` (branch `dev`, working tree clean when read). Where the vendored copy and the library disagree, the library wins.

## A. Core.React.UI vs the vendored copy (ClaudeP)

### File level

Result of a byte comparison of every copied file: **no disagreement.** All 41 files under
`claudep/.../src/ui/app/src/design-system/` (everything except the copy's own `README.md` and root
`index.ts`) are identical to the same path in `Timebase.Core.React.UI/src` at `137b848b9`. That is the
commit the snapshot README records. The tokens, tone records, button, select and the rest read the same in both.

The copy will drift as soon as the library moves past that commit. It is pinned, not live.

### Coverage

| | Library | Vendored copy |
|---|---|---|
| Component folders under `components/ui/` | 102 | 22 (badge, button, card, chart-area, confirm-dialog, icon-button, icons, input, kpi-card, legend-chip, number-field, page-header, panel-tabs, search-input, select, sidebar, status-dot, switch, theme-switcher, toast, tone-dot, tooltip) |
| Sidebar components | 10 (shell, brand, group, nav button, nav item, support, instance, footer, user menu, auth footer) | 3 (`timebase-sidebar`, `timebase-sidebar-group`, `timebase-nav-button`) |
| Shell | `TimebaseAppShell`, `TimebaseTopbar`, `TimebaseTopbarActions`, `TimebaseWorkspaceShell`, `TimebaseTabStrip` | None. ClaudeP wrote its own `Shell`, `TopBar`, `Sidebar`, `Brand`, `PageFrame` |
| Not vendored, but the patterns need them | Menu, Accordion, DataGrid, CopyButton, SelectGroup, ErrorCard, detail-layout, Drawers | ClaudeP built `DataTable`, `SegmentedControl`, `Callout`, field rows, native `<details>` instead |

### Shell, ClaudeP against the library

These are not file differences. They are places where ClaudeP's own code makes a different choice than the library's shell.

| Aspect | Library (`TimebaseAppShell`, `TimebaseTopbar`, `TimebaseSidebar`) | ClaudeP |
|---|---|---|
| Root height | `h-screen w-screen overflow-hidden` | `h-dvh` |
| Topbar spacing | `gap-4 px-[18px]` | `gap-2 px-3 sm:px-4` |
| Topbar content | Crumbs, pulse strip, search, actions | Sidebar toggle, page action slot, theme switcher, log out |
| Sidebar toggle | In the sidebar (`togglePos`, default `header`) | `togglePos="footer"` is set, but the app renders its own footer instead of `TimebaseSidebarFooter`, so the sidebar draws no toggle. The toggle is in the topbar. |
| Brand | `TimebaseSidebarBrand` and `TimebaseLogo` | Own `Brand`: 26px image and a `font-display text-sm` name |
| Sidebar footer | Version or user tile | A status block (dot, label, Pause button, note) |
| Narrow screens | None | Below 760px the sidebar is a drawer |
| Page container | `p-7`, full width (`DETAIL_PAGE_CONTENT`) | `max-w-content` (1600px) and `px-4 sm:px-6 pt-4 sm:pt-5 pb-10` |
| Page actions | A toolbar row in the page | A portal into the topbar slot |
| Theme boot | Inline script in `index.html` | A module (`theme/boot.ts`), because the CSP forbids inline scripts |
| Fonts | `@fontsource` imports, Inter 400, 500, 600, mono 400 and 500 | Hand-written `@font-face`, Inter 400 and 500, mono 400 only |
| RAC imports | Apps do not import RAC (`docs/Primitives.md`) | Seven files import RAC directly: `Form` in `Apps.tsx`, `RemoteLogin.tsx` and `SettingsSection.tsx`; `Table` in `DataTable.tsx` and `sort.ts`; `ToggleButtonGroup` in `SegmentedControl.tsx`; `FieldError`, `NumberField`, `Switch` and `TextField` in `fields.tsx` |

Resolution in this spec: the library's convention is the standard; ClaudeP's choices are marked
`APP CHOICE` where described, and the ones that are good candidates for the library are listed in
[07-proposals.md](07-proposals.md) as proposals (narrow-screen drawer, section headings, field rows,
short table, notice, code block, relative time, empty state).

## B. Known gaps in the library itself

Found while reading. They are the library's, not the apps'. Reviewers flag them and do not fail an app for them.

| # | Gap | Evidence |
|---|---|---|
| 1 | Light-mode `primary-foreground` on `primary` is white on `#31AEE4`: 2.53:1 (measured). Light `destructive-foreground` on `destructive` is 3.76:1. | `styles/tokens.css`, `button/timebase-button.tsx` |
| 2 | Tone text on a light card fails text contrast: `success` 1.80, `warning` 2.01, `info` 1.60, `destructive` 3.51 (measured, opaque card behind). Badges draw it on a translucent tint, which was not measured. | `lib/tone-classes.ts`, `styles/tokens.css` |
| 3 | The focus ring is 25% of the ring colour. Measured against the card behind it: 1.23:1 light, 1.79:1 dark. It is the only focus indicator on buttons, nav rows and tabs, so it is under the 3:1 non-text minimum. Inputs add a full-colour border (2.36:1 light). | `--tb-shadow-ring` |
| 4 | Dark `destructive-foreground` (`#F2F2F2`) on `destructive` is 3.36:1. The button avoids it by using `primary-foreground` (`#121214`) for its label, which is a different token than the colour file lists. | `tokens.css`, `timebase-button.tsx` |
| 5 | Button hover uses `var(--tb-blue-600)` and `var(--tb-green-800)` primitives directly, against the library's rule that components use semantic tokens. In dark mode the `primary-hover` token is `#4CD6F5` but the button hovers to `#1B81AD`. | `timebase-button.tsx` variants |
| 6 | The tab strip draws an unread dot with `bg-amber-500`, a Tailwind palette class. | `tab-strip/timebase-tab-strip.tsx` line 912 |
| 7 | The toast stack imports `lucide-react` glyphs directly and sizes its close glyph 10px, outside the `TimebaseIcon` registry and size table. | `toast/timebase-toast-stack.tsx` |
| 8 | The nav `badge` wrapper is `aria-hidden`, so an attention dot cannot be heard. Collapsed, the dot is the only signal. | `sidebar/timebase-nav-button.tsx` |
| 9 | The nav components set `data-active` only, not `aria-current`. Apps add it by hand. | `timebase-nav-button.tsx`, `timebase-nav-item.tsx` |
| 10 | `TimebaseIconButton`'s `tooltip` is optional, so an icon-only button without a tooltip type-checks. | `icon-button/timebase-icon-button.tsx` |
| 11 | RESOLVED in the spec, OPEN in the library. `--font-sans` lists Poppins before Inter. The brand kit and the archived brand system both say Inter is the body face. The spec follows the brand. The library needs a one line change. | `tokens.css`, `brand/typography.md`, archived `brand-system.md` |
| 12 | RESOLVED. The brand kit palette names Charcoal `#1F1F23` as the dark-mode background. The archived brand system's role table and the library both use `#121214` for the dark page. The spec keeps `#121214`. The brand palette wording should be corrected. | `brand/colors/palette.md`, archived `brand-system.md`, `tokens.css` |
| 13 | Two tone vocabularies: `Tone` uses `error`, `TimebaseKpiTone` uses `destructive`, token names use `destructive`. | `lib/tone-classes.ts`, `kpi-card/timebase-kpi-card.tsx` |
| 14 | Dot sizes differ: `TimebaseToneDot` is 8px, `TimebaseHealthDot` is 7px, nav badge dots are 6px. | `tone-dot`, `health-dot`, Historian `app-sidebar` and instance button |
| 15 | The FOUC boot colour for light is `#F2F2F2`, the light page token is `#FFFFFF`. A brief mismatch before the stylesheet loads. | library README, `tokens.css` |
| 16 | Two comments disagree about the tooltip delay: the tooltip says the RAC default is 1500ms, the icon button says "Base UI's 600". | `tooltip/timebase-tooltip.tsx`, `timebase-icon-button.tsx` |
| 17 | A source comment records that `property-editor-atlas` still carries copies of the select popover, listbox and empty-state classes that `select-surface.tsx` now owns. | `select/select-surface.tsx` header |
| 18 | The library README links `docs/UI-STANDARD.md` and a `brand-system.md`. `UI-STANDARD.md` is still missing. `brand-system.md` exists only as an archived copy at `timebase-reviews/main/archive/dev-1.4/Timebase.Atlas/docs/design/brand-system.md`, from the dev-1.4 era. | `README.md`, `tokens.css` header |

## C. Not verified

- Nothing was run or rendered. Every dimension is read from class strings, not measured in a browser. No screenshots were taken.
- Contrast ratios in [02-color.md](02-color.md) and the focus ring figures are my calculation from the token hex values, with opaque backgrounds. Translucent tints (badge fills, the 25% ring over a mixed surface) were approximated by blending, not measured in a browser.
- Not checked against a build: the value of Tailwind's bare `rounded`; that `font-sans` (Poppins first) really renders body text in Poppins; whether the installed `lucide-react` adds `aria-hidden` to icons by default; the tooltip's real default delay.
- Not found: `docs/UI-STANDARD.md` and the ADRs the library README cites. `brand-system.md` was found as an archived copy. The spec does not depend on any of them.
- Not read in depth: the tab strip, workspace shell internals, property editor, data grid, floating drawer, support menu contents, notification bell, Ask Gage panel, and the other Timebase UIs (Explorer, Portal, Pulse, Atlas, Collector, Bridge, Connect). They are covered here only where Historian or the library comments describe them.
- The token lint rule was run once against a scratch file, not against a real app. It may flag existing code.
- The brand kit (`brand/` in this repo) is now on `main`, and this spec links to it.
- The ClaudeP worktree is being edited by another session. What I read may have changed.
