# 01 Shell

The frame around every page: sidebar, topbar, page container. Dimensions below were read from the
library and checked against the Historian app and the ClaudeP shell.

Two shell shapes exist today. Pick one per app.

| Shape | Used by | Top bar | Content |
|---|---|---|---|
| Workspace shell | Historian and the other Timebase apps | The tab strip row (no separate topbar) | Tabs, optional split, optional right side panel |
| Page shell | ClaudeP | `TimebaseTopbar`-style bar above the page | One page at a time, centred container, one scroll area |

Both use the same sidebar.

```
Workspace shell                               Page shell
+--------+---------------------------+        +--------+---------------------------+
| brand  | tab strip + trailing      |        | brand  | topbar                    |
| (h-14) | (56px row, aligned)       |        | (h-14) | (h-14)                    |
+--------+---------------------------+        +--------+---------------------------+
| nav    | pane content (p-7)        | panel  | nav    | scroller                  |
|        |                           | (right)|        |   centred container       |
| ...    |                           |        | ...    |     page header           |
| support|                           |        | footer |     content               |
| footer |                           |        |        |                           |
+--------+---------------------------+        +--------+---------------------------+
```

## Root

| Part | Classes | Source |
|---|---|---|
| Workspace shell root | `flex h-screen w-screen bg-background text-foreground overflow-hidden` | `components/ui/app-shell/timebase-app-shell.tsx` |
| Main column | `flex-1 flex flex-col min-w-0 min-h-0`; topbar in a `shrink-0` wrapper | same |
| Main region | `<main class="flex-1 min-h-0 overflow-hidden flex flex-col">` | same |
| Right side panel | Optional. A resizable column to the right of the main column, full height. Width is controlled, min 320px (default), divider has an aria-label. It pushes the workspace, it never overlays it. | same |
| Overlay slot | Peer of sidebar and main, for drawers and the toast stack. No padding, no clipping. | same |
| Page shell root (ClaudeP) | `flex h-dvh bg-background text-foreground` (APP CHOICE: `h-dvh`) | ClaudeP `shell/Shell.tsx` |

Rules:

- The root is exactly one viewport tall and never scrolls. `main` is `overflow-hidden`, so a page owns its own scrolling (see Scroll rules).
- Every flex ancestor between the root and a scrolling region carries `min-h-0` (and `min-w-0` horizontally). Without it the region grows instead of scrolling.

## Sidebar

`Source: components/ui/sidebar/timebase-sidebar.tsx` and siblings in the same folder.

### Dimensions

| Part | Value |
|---|---|
| Width expanded | 216px (`w-[216px]`) |
| Width collapsed | 58px (`w-[58px]`) |
| Collapse transition | `transition-[width] duration-base ease-standard`, which is 200ms on `cubic-bezier(0.2, 0, 0, 1)` |
| Surface | `bg-sidebar`, right edge `border-r border-sidebar-border` |
| Brand row | `h-14` (56px), `border-b border-border`, `gap-2.5`; expanded `px-3.5`, collapsed `justify-center px-0` |
| Nav body | `flex-1 min-h-0 overflow-y-auto p-2 flex flex-col gap-0.5` |
| Pinned region (Support, Instance) | `flex-shrink-0 px-2 py-2 border-t border-border`, outside the scrolling nav |
| Footer slot | Below the pinned region. Version footer: `px-2.5 py-2.5 border-t border-border` |

The brand row is 56px so it lines up with the topbar and the tab strip row. Both bottom borders must
read as one continuous line across the window. Keep them equal.

### Brand

- `TimebaseSidebarBrand product="..."` renders the logo at size `sm` with the product as the sub-label.
- Collapsed it renders the icon only and drops the sub-label.
- An app with its own mark (ClaudeP) uses a 26px image plus a `font-display text-sm font-semibold tracking-tight` name, and drops the name when collapsed. APP CHOICE.

### Collapse toggle

`togglePos` picks where the control lives. Default `header`.

| `togglePos` | Control | Source detail |
|---|---|---|
| `header` (default) | 22x22 `rounded` ghost button with a chevron-left (14px) in the brand row, right side. Collapsed, the whole top 28px strip becomes an invisible "Expand menu" button (`absolute top-[14px] left-0 right-0 h-7`). | `HeaderToggle`, `HeaderToggleCollapsed` |
| `edge` | 22x22 round button straddling the right edge, `absolute top-[42px] -right-[11px] z-[5]`, `bg-card border border-border shadow-sm`, chevron 13px | `EdgeToggle` |
| `footer` | 24x24 button in `TimebaseSidebarFooter`, icons `sidebarCollapse` and `sidebarExpand` at 15px; collapsed it floats at `right-1.5 bottom-1.5` | `timebase-sidebar-footer.tsx` |
| `hover` | 6x44 sliver on the right edge, `opacity-0` until the sidebar is hovered, then 60% | `HoverToggle` |

Every toggle has an `aria-label` ("Collapse menu", "Expand menu") and a `title`. Historian uses `header`.
ClaudeP uses `footer` mode but renders its own footer and puts the toggle in the topbar instead
(APP CHOICE, see Topbar).

### Nav items

`TimebaseNavButton` (an action, native `<button>`) and `TimebaseNavItem` (a link). Same look.

| Part | Value |
|---|---|
| Row | `relative flex items-center gap-2.5 rounded text-sm font-medium leading-tight`, full width |
| Padding | Expanded `px-2.5 py-2`. Collapsed `p-2.5 justify-center` |
| Icon size | 17px on `TimebaseNavButton` (`iconSize` default), 18px on `TimebaseNavItem` |
| Label | `truncate`, hidden when collapsed |
| Inactive | `text-muted-foreground hover:bg-accent hover:text-foreground` |
| Active | `bg-accent text-foreground` plus the indicator bar |
| Indicator bar | `absolute left-0 top-2 bottom-2 w-[2.5px] rounded bg-primary`, `aria-hidden` |
| Transition | `transition-colors duration-fast ease-standard` (120ms) |
| Focus | `focus-visible:outline-none focus-visible:shadow-ring` |
| Collapsed tooltip | `aria-label={label}` and `title={label}`, a native tooltip |
| Badge slot | `badge` prop. `absolute right-1`. Expanded: `top-1/2 -translate-y-1/2`. Collapsed: `top-1` (corner of the icon) |
| External links | `TimebaseNavItem` with an absolute href or `target="_blank"` renders a plain `<a>` with `rel="noopener noreferrer"`. Use the `external` icon. |

Rules:

- Mark the current item with `aria-current="page"` as well as `active` (ClaudeP does; the library only sets `data-active`).
- The badge slot holds an attention dot (`TimebaseToneDot`, `h-1.5 w-1.5` in Historian) or a count (`TimebaseBadge`, counts above 99 read `99+`). Collapsed, it is the only thing left that can say something needs attention.
- The badge wrapper is `aria-hidden`. State that matters must also be reachable another way (tooltip content on the row, or the page the item opens). See the appendix.
- Order inside a group: the working surface first, then the rest alphabetically. Historian's Status group puts Logs first, then the rest alphabetically.

### Groups

`TimebaseSidebarGroup`: `flex flex-col gap-0.5`.

| Part | Expanded | Collapsed |
|---|---|---|
| Label | `px-2.5 font-sans font-semibold text-[10px] uppercase tracking-[0.1em] text-muted-foreground`; first group `pt-1.5 pb-1`, others `pt-3.5 pb-1` | Replaced by a hairline `h-px bg-border opacity-70 mx-2.5`; first group `my-1.5`, others `mt-2.5 mb-1.5` |
| First group | `isFirst`, no label (the top group is unlabelled) | |

Labels are singular nouns: "Status", "Developer", "Configure". The same name on every app that has the group.

### Pinned region and footer

- `support` slot: `TimebaseSidebarSupportButton`. Always visible, never scrolls away. Its menu rail expands upward.
- `instance` slot: `TimebaseSidebarInstanceButton`, directly below Support in the same region and under the same single separator. It carries the licence attention dot. It is a button, not a menu, so the dot is visible without opening anything.
- `footer` slot: identity or version.
  - `TimebaseAppAuthFooter`: user tile (avatar 22px, name, email) with a Sign out menu. When auth is disabled it is a static "Auth disabled" tile with "Administrator" and no menu.
  - `TimebaseSidebarFooter`: "Version" label (10px, uppercase, `tracking-[0.08em]`) and a mono value (`font-semibold text-[12.5px]`); collapsed, a short form at `text-[10.5px]`.
- Footer status block (APP CHOICE, ClaudeP `shell/Sidebar.tsx`): `border-t border-border px-2.5 py-2.5`. A row `flex min-h-7 items-center gap-2 text-sm font-medium` with a `TimebaseStatusDot` and a label; under it `text-xs leading-snug text-muted-foreground mt-1.5`. Collapsed it is the dot alone with an `aria-label`. Use this pattern for a live "Running / Paused" state, with the tone mapping good to success, warn to warning, bad to error.

### Narrow screens

The library has no narrow behaviour. ClaudeP defines one, so it is the working reference. APP CHOICE,
candidate to promote to the library.

| Aspect | Value |
|---|---|
| Breakpoint | `(max-width: 760px)` via `matchMedia` |
| Below it | The sidebar is not on the page. The topbar toggle opens it as a drawer. |
| Drawer | Backdrop `fixed inset-0 z-30 bg-overlay`, click to close. Panel `fixed inset-y-0 left-0 z-40 shadow-lg`, always expanded (216px). |
| Closing | Choosing a nav item closes it. The toggle closes it. |
| Keyboard | `Ctrl+\` or `Cmd+\` toggles the sidebar (or the drawer) on any width. |

Source: ClaudeP `shell/useSidebar.ts`, `shell/Shell.tsx`.

## Topbar

### Page shell topbar

`Source: components/ui/topbar/timebase-topbar.tsx`

| Part | Value |
|---|---|
| Element | `<header data-component="topbar">`, `h-14`, `flex items-center gap-4 px-[18px] flex-shrink-0` |
| Surface | `bg-card`, `border-b border-border` |
| `crumbs` slot | Left. `flex items-center gap-1.5 text-[13px]`. The consumer draws separators. |
| `pulseStrip` slot | After the crumbs, `pl-3.5 ml-0.5 border-l border-border`. `TimebaseTopbarPulseStrip` is hidden below 1180px (`@media(min-width:1180px)`), `gap-3.5`. |
| Spacer | `flex-1` |
| `search` slot | Right, before actions |
| `actions` slot | Rightmost, `flex items-center gap-2` |

What lives in it: breadcrumbs, live KPI chips, a search field, global actions, the theme switcher.
What does not: the page title (the page header owns it), navigation (the sidebar owns it), page
content, primary page actions that belong with the content.

Standard trailing cluster, `TimebaseTopbarActions`: `flex items-center gap-1 px-1.5`, in this order:
split toggle, `TimebaseThemeSwitcher`, notification bell, Ask Gage button (hidden while its panel is open).
App-specific leading actions go in `children`, before the cluster.

ClaudeP's bar (APP CHOICE, `shell/TopBar.tsx`) is narrower in purpose: `flex h-14 flex-none items-center gap-2 border-b border-border bg-card px-3 sm:px-4`,
holding the sidebar toggle (`TimebaseIconButton`, size `iconsm`, tooltip "Hide sidebar (Ctrl+\)"), a spacer,
the page-action slot (`flex items-center gap-1`), the theme switcher, and a ghost small "Log out" button when the session is remote.

### Workspace shell: the tab strip is the topbar

Historian has no `TimebaseTopbar`. The workspace shell renders one row per pane: `[tab strip] + [content]`.

| Prop | Historian value | Meaning |
|---|---|---|
| `tabBarHeight` | `56` | Row height in px. Set to the sidebar brand row height so the borders align. |
| `stripClassName` | `"bg-sidebar"` | Surface of the strip row and the trailing cluster. The default is `bg-card`; tabbed apps use `bg-sidebar`. |
| `trailing` | `TimebaseTopbarActions` | Right end of the strip row. In split mode it rides the right strip. |

Source: `components/ui/workspace-shell/timebase-workspace-shell.tsx`, Historian `layouts/main-layout.tsx`.

## Page container

Two models. Choose by app type, do not mix them in one app.

| Model | When | Container | Scroll |
|---|---|---|---|
| Workspace page | Tabbed apps, catalogs, lists that fill the pane (Historian) | `flex flex-col h-full p-7` (28px all sides), full pane width, no max width | The page does not scroll. Its list region is `min-h-0 overflow-auto` and scrolls internally. Header and toolbar are `shrink-0`. |
| Document page | Single-page apps with stacked sections (ClaudeP) | `mx-auto w-full max-w-content px-4 pb-10 pt-4 sm:px-6 sm:pt-5` | The scroller `flex-1 overflow-y-auto` above it scrolls the whole page. Scroll to top on page change. |

The library states the convention in code: `DETAIL_PAGE_CONTENT = "p-7"`, "full width with p-7, matching
every app's page convention", used as `<div className={DETAIL_PAGE_CONTENT}>` inside a
`h-full overflow-y-auto` scroller (`components/ui/detail-layout/detail-layout.tsx`). So a long
single-thing page in a tabbed app is: scroller `h-full overflow-y-auto`, content `p-7`, full width.
That is the library's standard. The library defines no max width.

`APP CHOICE`: the document page is ClaudeP's own. It defines `--container-content: 100rem` (1600px)
in its own `index.css` `@theme` and uses responsive padding instead of `p-7`. It differs from the
library convention, see the appendix. Source for Historian: `pages/datasets-page.tsx`,
`pages/collectors-page.tsx`, `pages/logs-page.tsx`.

Responsive padding for the document page: 16px side and top at phone width, 24px side and 20px top from
the `sm` breakpoint (640px), 40px bottom always.

## Scroll container rules

- Exactly one scroll owner per view. The root, `main` and the topbar never scroll.
- Workspace page: the region that grows (a table, list, log) is the scroller. Everything above it is `shrink-0`.
- Document page: the scroller is the single `overflow-y-auto` block under the topbar. The topbar stays put.
- Wide tables scroll horizontally inside their own wrapper (`overflow-x-auto`), never the page.
- Scrollbars take the global themed recipe, see [08-global-styles.md](08-global-styles.md). Do not restyle them per component.
- A region that hides its own scrollbar (the tab strip) sets `scrollbar-width: none` on that element only.

## Page header and the action slot

`TimebasePageHeader`, `Source: components/ui/page-header/timebase-page-header.tsx`.

| Part | Value |
|---|---|
| Title | `<h1 class="font-display font-semibold text-[28px] tracking-tight m-0">` |
| Description | `text-[13px] text-muted-foreground mt-1 max-w-[780px]` |
| `trailing` | Text on the title's baseline: a count, a word. Not a bordered box. |
| `badge` | A pill or small cluster, centred on the title's line (or on the trailing text's line when it follows some) |
| Outer | `<header class="flex-shrink-0">`. No bottom margin built in. |

Rules:

- One page header per page, at the top, the only `h1`.
- The spacing to the next block is the consumer's: `className="mb-4"` (16px) is the standard when a toolbar or content follows. Historian and ClaudeP both use it.
- Description is one or two sentences, plain, says what the page shows.

Where a page puts its actions:

| Slot | Use | Example |
|---|---|---|
| Toolbar row under the header | The page has a filter. Filter left, primary action right: `mb-3 flex shrink-0 items-center justify-between gap-3`, filter wrapper `w-full max-w-xs` (320px) | Historian Datasets: filter plus "Add dataset" |
| Topbar action slot | Page-level actions in a page-shell app. The page renders into the topbar through a React portal (`TopActions`) into the slot element (`ActionsSlot`). APP CHOICE. | ClaudeP |

Gate admin-only actions by hiding them for people who cannot use them. Do not show a disabled button
with no explanation.
