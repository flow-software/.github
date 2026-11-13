# 06 Base components

Every component here is in `@timebase/react-ui` (`components/ui/<name>/`). Behaviour and ARIA come
from React Aria Components (RAC). Styling is Tailwind utilities over `--tb-*` tokens, composed with
`cn()` (clsx plus tailwind-merge) and `cva` for variants. Source paths are under `Timebase.Core.React.UI/src`.

Conventions that hold for all of them:

| Rule | Detail |
|---|---|
| Prefix | Exports are `Timebase*` |
| Base | RAC, imported by sub-path (`react-aria-components/Button`). Only the library imports RAC. Apps use the wrappers. |
| API style | Timebase props (`value`, `onChange`, `invalid`, `disabled`, `onClick`), not RAC's (`selectedKey`, `isInvalid`, `onPress`). The wrapper translates. |
| State on the DOM | `data-component`, `data-variant`, `data-size`, `data-tone`, `data-active`, `data-invalid` so tests assert attributes, not classes |
| Variant states | Use the RAC Tailwind plugin variants (`selected:`, `entering:`, `invalid:`, `focus-visible:`), not hand-written `data-[...]` |
| Focus | `focus-visible:outline-none focus-visible:shadow-ring` on every interactive element |
| Disabled | `opacity-50`, `cursor-not-allowed` or `pointer-events-none` |
| Provider-less | No theme, i18n or error provider in the library. The app wires them. |
| `data-id` | A component an in-product guide may point at accepts and forwards `data-id` |

## Button

`button/timebase-button.tsx`. Purpose: every labelled action.

| Variant | Class recipe | Use |
|---|---|---|
| `primary` (default) | `bg-primary text-primary-foreground hover:bg-[var(--tb-blue-600)]` | The one main action of a view or dialog |
| `secondary` | `bg-secondary text-secondary-foreground hover:bg-[var(--tb-green-800)]` | Rare secondary call to action |
| `outline` | `border border-border bg-transparent text-foreground hover:bg-accent` | Cancel, standard secondary actions |
| `ghost` | `bg-transparent text-muted-foreground hover:bg-accent hover:text-foreground` | Low emphasis, toolbar, row actions |
| `subtle` | `bg-muted text-foreground hover:bg-accent` | Quiet filled |
| `destructive` | `bg-destructive text-primary-foreground hover:bg-destructive/90` | The confirm button inside a destructive dialog |
| `link` | `bg-transparent text-primary underline-offset-4 hover:underline` | Inline link-style action |

| Size | Height | Padding and text |
|---|---|---|
| `sm` | 28px | `px-2.5 text-xs rounded` |
| `default` | 36px | `px-3.5 text-sm rounded-md` |
| `lg` | 44px | `px-5 text-base rounded-lg` |
| `icon` | 36 x 36 | `p-0` |
| `iconsm` | 28 x 28 | `p-0` |

Props: `variant`, `size`, `icon` (a registry name, shown before the label), `loading`, `disabled`,
`onClick`, `type` (default `"button"`). Base: `inline-flex cursor-pointer items-center justify-center gap-2 whitespace-nowrap font-medium transition-colors duration-fast ease-standard`.

States: hover per variant; focus ring; `disabled` is `opacity-50 pointer-events-none`; `loading`
replaces the icon with a spinning `refresh`, sets `data-loading`, and RAC marks it pending.

Do:
- One primary button per view or dialog. Everything else is `outline`, `ghost` or `subtle`.
- Label with a verb, sentence case, one to three words: "Add dataset", "Save", "Cancel".
- Use `loading` for an async action. The button keeps its width and ignores presses.
- Set `type="submit"` for a form's submit button.

Do not:
- Use `destructive` as the trigger that opens a confirmation. The trigger is `TimebaseDeleteButton` (outline, red text). The red fill belongs on the confirm button.
- Put an icon-only action in `TimebaseButton`. Use `TimebaseIconButton`.
- Pass a raw colour class to change the look. Add a variant to the library.
- Rely on the `title` attribute for meaning. Use a tooltip.

Related: `TimebaseDeleteButton` (`button/timebase-delete-button.tsx`): outline button, `text-destructive hover:bg-destructive/10`, trash glyph 13px, bare verb label (`"Delete"` by default, `"Remove"` for detaching). The visible label is a bare verb; the object goes in `aria-label` when it needs one. Default size `sm`.

## IconButton

`icon-button/timebase-icon-button.tsx`. Purpose: an action shown as a glyph alone.

| Prop | Notes |
|---|---|
| `icon` | Registry name, required |
| `aria-label` | Required by the type |
| `size` | `icon` (36) or `iconsm` (28). Default `icon`. |
| `variant` | Default `ghost` |
| `tooltip`, `tooltipSide`, `tooltipDelay` | Wraps the button in `TimebaseTooltip` |
| `loading` | Spinner replaces the glyph |

Glyph: 16px stroke 1.5 at `icon`, 12px stroke 2 at `iconsm`.

Do: always set `aria-label` and `tooltip`; keep them the same words; use `iconsm` in dense bars and cards.
Do not: ship one without a tooltip; use one for a primary action that needs words; hide an essential action behind an unlabelled glyph.

## Select

`select/timebase-select.tsx`, `select/select-surface.tsx`. Purpose: choose one of a closed list. Built on RAC `Select`.

| Prop | Notes |
|---|---|
| `value`, `onChange`, `options` | `options` are `{ value, label, disabled?, hint? }`. `value` null shows the placeholder. |
| `density` | `comfortable` (h-9, 13px) or `compact` (h-8, 12.5px) |
| `mono` | Monospace value, for identifiers |
| `invalid` | Destructive border |
| `ariaLabel`, `ariaLabelledBy` | One is required, the control has no visible label of its own |
| `defaultOpen` | Mount open, for a select the user was given |
| `className` | Applied to the trigger (set width here, for example `w-56`) |

Look: trigger `w-full rounded-md border border-input bg-input-surface`, chevron 12px; open or focused
shows `border-ring shadow-ring`; popover `bg-popover border border-border rounded-[10px] p-1 shadow-lg`,
offset 4, `min-w` the trigger width, `max-h` the available height; option row
`px-2.5 py-2 text-sm font-medium text-muted-foreground`, focused `bg-accent text-foreground`, selected `text-foreground`.
A `hint` is a muted 10.5px uppercase chip pinned right and stays out of the accessible name.
With no options it renders a static, disabled trigger instead of mounting RAC (avoids a render loop).

Related: `TimebaseComboBox` (searchable, caller filters), `TimebaseEnumSelect`, `TimebaseDurationUnitSelect`.

Do: use for 4 or more options; provide a visible label next to it and point `ariaLabelledBy` at it; show a placeholder when nothing is chosen.
Do not: use for 2 or 3 options (use a segmented control); use for an on or off setting (use a switch); build a custom dropdown.

## Input

`input/timebase-input.tsx`. Purpose: single-line text.

Look: `w-full px-3 py-2 rounded-md border border-input bg-input-surface text-foreground text-sm leading-snug`,
placeholder `text-muted-foreground`. Focus: `border-ring shadow-ring`. Invalid: `border-destructive` and a destructive 25% ring.
Disabled: `opacity-50 cursor-not-allowed`. The `invalid` prop sets `data-invalid` and `aria-invalid`.

Do: always give it a label (visible, or `aria-label`); set `type`, `autoComplete` and `spellCheck` honestly.
Do not: place a field on `bg-input-surface` inside another field-coloured container; use placeholder as the label; show an error by colour alone, add text.

## SearchInput

`search-input/timebase-search-input.tsx`. Purpose: filter a list. An `Input` of `type="search"` with a 14px `search` glyph at `left-3` and `pl-9`. Default placeholder "Search...". `containerClassName` sizes the wrapper.

Do: put it left in the toolbar row at `max-w-xs` (320px); filter as the user types; give it an `aria-label` ("Filter datasets").
Do not: use it for entering a value that is submitted.

## NumberField

`number-field/timebase-number-field.tsx`. Purpose: numeric entry. Controlled `value: number | null` (null is empty), `onChange`, `min`, `max`, `step`, `format` (default raw integer, no grouping, up to 20 fraction digits), `ariaLabel` or `ariaLabelledBy`. Same look as `Input`.

Do: set `min` and `max` where they exist; pass a `format` for decimals.
Do not: use it for identifiers (ports are fine, phone numbers are text); omit the label.

## Switch

`switch/timebase-switch.tsx`. Purpose: an on or off setting that applies immediately or on form save.

Look: track 36 x 20 (`w-9 h-5 rounded-full`), off `bg-switch-track`, on `bg-primary`; thumb 16px `bg-switch-thumb` with a small shadow, travels from `left-0.5` to `left-[18px]`. Focus ring, `disabled:opacity-50`.
Props: `checked`, `onCheckedChange`, `disabled`, `ariaLabel`, `ariaLabelledBy`, `data-id`.
The component has no built-in label. Put a text label beside it and wire `ariaLabelledBy`, or build the labelled-row pattern in [07-proposals.md](07-proposals.md).

Do: label with the thing being enabled, as a state: "Pause requests", "Require API key".
Do not: use for an action (use a button); use for a choice among options (use a select or segmented control); use a switch for something that needs confirmation to change.

## Badge

`badge/timebase-badge.tsx`. Purpose: a small static identifier or count. Tones `neutral | info | success | warning | error` via `TONE_FILL`.
Look: `inline-flex items-center gap-1 px-2 py-[2px] rounded-pill text-[11px] font-medium tracking-[0.01em] border`.

Do: keep to one to three words or a number; counts above 99 show `99+`; pair a tone with words.
Do not: use as a button; carry state by colour alone; use for a long sentence.

## StatusDot and ToneDot

`status-dot/timebase-status-dot.tsx`, `tone-dot/timebase-tone-dot.tsx`.

| Component | Purpose | Notes |
|---|---|---|
| `TimebaseToneDot` | The dot alone. 8px (`size-2`), `TONE_DOT` colour and 3px glow. `pulse` animates. | Resize with `className` (`h-1.5 w-1.5` in a nav badge slot). It has no accessible name: pass one or pair it with text. |
| `TimebaseStatusDot` | Dot plus optional label as `role="status"`. | `tone`, `label`, `pulse`. Label is `text-xs text-muted-foreground`. A string label becomes the `aria-label`. Dot-only needs `aria-label` passed in. |

Do: map your own states to a tone yourself; pulse only a transitional or live state.
Do not: show a dot with neither label nor accessible name; use the dot colour as the only status cue; pulse more than one thing at a time.

## Card

`card/timebase-card.tsx`. Purpose: a bordered container for a group of related content.
Look: `bg-card text-card-foreground border border-border rounded-lg`, no shadow. Padding `none | md (p-5) | lg (p-6)`.
Props: `title` (`font-display font-medium text-base tracking-tight`), `sub` (13px muted), `action` (right), `interactive`, `onClick`.
The header gets `mb-4` only when there is a body. `interactive` makes the card `role="button"` with `tabIndex=0` and Enter or Space activation.

Do: use for a self-contained unit (a summary, a form group, a panel); put the card's one action in `action`.
Do not: nest a card in a card; make a card interactive when it holds other interactive controls; add a shadow.

## KpiCard

`kpi-card/timebase-kpi-card.tsx`. Purpose: one headline number in a strip.
Parts: `label` (11px, uppercase, `tracking-wider`, muted), `value` (`font-display text-2xl font-semibold tabular-nums`), optional `hint` (11px muted), optional `footer` (`mt-2`). `tone`: `default | success | warning | destructive` colours the value only. Note this tone union spells `destructive`, not `error`.
Layout: a responsive grid, `grid-cols-[repeat(auto-fit,minmax(9.5rem,1fr))] gap-2.5` (ClaudeP, APP CHOICE: 9.5rem is its `--claudep-tile-min`).

Do: keep the value short and formatted (`1.2k`, `$4.20`); explain it in the hint.
Do not: colour a value to say "bad" without a word in the hint; put more than one number in a card.

Related: `TimebaseStatusCard` (a word plus facts), `TimebaseMeter` (X of Y, tone-neutral on purpose).

## PageHeader

See [01-shell.md](01-shell.md). `font-display font-semibold text-[28px] tracking-tight` title, optional description, `trailing`, `badge`. One per page.

## PanelTabs

`panel-tabs/timebase-panel-tabs.tsx`. Purpose: switch between sibling panels inside one surface (an editor drawer, a settings panel). Not document tabs (that is `TimebaseTabStrip`).
Parts: `Root` (`value`, `onValueChange`, `orientation`), `List` (`ariaLabel`), `Tab` (`value`, `danger`, `disabled`, `data-id`).
Look: list `flex gap-0 px-4 overflow-x-auto border-b border-border bg-transparent`, no fill of its own; tab `px-3.5 py-2.5 text-[12.5px] font-medium`, muted until hover or selected; selected shows a 3px `bg-primary` bottom bar that slides between tabs (`bg-destructive` and destructive text for `danger`).

Do: 2 to 7 short labels; a `danger` tab for a destructive group.
Do not: use for navigation between pages; put a card around the list; hard-code a fill.

## ConfirmDialog

`confirm-dialog/timebase-confirm-dialog.tsx`. Purpose: confirm an action. RAC `ModalOverlay`, `role="alertdialog"`.

| Prop | Notes |
|---|---|
| `open`, `onClose`, `onConfirm` | `onConfirm` may be async |
| `title`, `description` | Title 15px display semibold; description 13px muted, relaxed |
| `confirmLabel` (`Confirm`), `cancelLabel` (`Cancel`) | Use the verb: "Delete" |
| `tone` | `default` or `destructive`. Destructive uses the destructive button and focuses Cancel first. |
| `busy` | Locks both buttons, spinner on confirm, dismissal off |
| `confirmDisabled` | Disables Confirm while required input is invalid, Cancel and Esc still work |
| `children` | Body content between description and buttons, for example a required note |

Look: backdrop `bg-overlay` at z 200, panel z 201, `bg-card border border-border rounded-lg shadow-lg`, `w-[420px] max-w-[calc(100vw-32px)] p-5 flex flex-col gap-3`, buttons right-aligned `gap-2`, both `sm` (Cancel `outline`). Fades in over 120ms.

Do: say what will happen and to what in the title and description; name the record; make the confirm label the same verb as the trigger.
Do not: use for information (use a toast) or for a choice with more than two outcomes; stack dialogs; set a destructive confirm to default focus.

## Toast

`toast/timebase-toast-stack.tsx`, `toast/add-toast-context.ts`. Purpose: a transient result of an action.

Variants `info | success | warning | error`; the left bar and glyph take the tone (`info` uses primary). Card look: `min-w-[260px] max-w-[360px] rounded-[10px] bg-card border border-border border-l-[3px] shadow-lg`, `px-3 py-2.5`, glyph 15px, title 13px semibold, optional message 12px muted, ghost `iconsm` dismiss button with `aria-label="Dismiss"`.
Placement: fixed `bottom-5 right-5`, newest on top, `gap-1.5`, z 200. Region is `role="region"` `aria-label="Notifications"` `aria-live="polite"`; an error toast is `role="alert"`, the rest `role="status"`.
Lifetime: auto-dismiss after about 5s (exit starts at 4800ms).

Wiring: the shell calls `useToasts()` once, renders the single `TimebaseToastStack`, and provides `addToast` through `AddToastContext`. Everything below calls `useAddToast()`. One stack per app.

Do: title is the outcome in plain words ("Settings saved"); message only when it adds a reason or a next step; use `error` for failures.
Do not: mount a second stack; put an action button in a toast; rely on a toast alone for an error the user must act on (also show it inline); put long text in one.

## Tooltip

`tooltip/timebase-tooltip.tsx`. Purpose: a short label for a control that has no visible text, or a hint.
Look: `bg-popover border border-border rounded-md text-foreground font-sans text-xs leading-snug px-2 py-1 shadow-lg max-w-[280px]`, z 150, `pointer-events-none`, default side `top`, offset 6, align `center`.
Opens on hover and keyboard focus. `delay` in ms; the component's own comment gives the RAC default as 1500ms. `disabled` suppresses it. A raw host element child (a `<span tabIndex={0}>`) is wrapped in RAC `Focusable`.

Do: one short phrase; name the action for an icon-only control; add the shortcut in brackets ("Hide sidebar (Ctrl+\)").
Do not: put interactive content in it (it ignores the pointer); use it as the only place an essential fact lives; put a tooltip on a control that already shows the same text.

Related: `TimebaseFieldHelp` is an info button whose tooltip is the help text, with the text always in the accessibility tree and a trigger name "Help for <label>" (or "Why <label> is disabled", with a `lock` glyph, when it explains a disabled field). Use it beside a field label. `TimebaseInfoIcon` is the simpler `info` glyph with a tooltip.

## Sidebar parts

Anatomy and dimensions are in [01-shell.md](01-shell.md).

| Part | Purpose |
|---|---|
| `TimebaseSidebar` | The shell. `brand`, `children`, `support`, `instance`, `footer`, `togglePos`, `collapsed`, `onCollapsedChange`, `defaultCollapsed`. |
| `TimebaseSidebarBrand product="X"` | Logo plus product sub-label, icon only when collapsed |
| `TimebaseSidebarGroup` | A labelled block of nav rows. `isFirst` for the unlabelled top group. |
| `TimebaseNavButton` | A nav row that fires an action. `icon`, `label`, `active`, `badge`, `iconTone`, `iconSize`. |
| `TimebaseNavItem` | A nav row that is a link. External hrefs render a plain anchor. |
| `TimebaseSidebarSupportButton` | Pinned Support menu, expands upward, ships default Knowledge Base items |
| `TimebaseSidebarInstanceButton` | Pinned "Instance Info" entry with the licence attention dot |
| `TimebaseSidebarFooter` | Version display |
| `TimebaseSidebarUserMenu`, `TimebaseAppAuthFooter` | Identity tile with a Sign out menu, or a static "Auth disabled" tile |
| `useTimebaseSidebar()` | `{ collapsed, toggle, togglePos }`, only inside `TimebaseSidebar` |

Do: use the parts, in this order: brand, groups, support, instance, footer; keep nav labels to one or two words.
Do not: write a local sidebar; put Support in the nav body; add a second separator between support and instance.

## ChartArea

`chart-area/timebase-chart-area.tsx`. Purpose: the substrate for a custom chart. Owns one `<svg>`, builds d3 scales from numeric domains, draws optional gridlines and a value axis, and hands the scales to its children.

| Prop | Notes |
|---|---|
| `xDomain`, `yDomain` | `[min, max]` numbers (epoch ms for a `time` scale) |
| `xScale` | `time` or `linear` (default) |
| `gridX`, `gridY` | Line count or `false` (default `false`) |
| `yTickFormat`, `yTicks` | Value-axis labels in the left gutter. The caller reserves room with `padding.left`. |
| `padding` | `{ top, right, bottom, left }` px around the plot interior (default all 0) |
| `width`, `height` | px or `"auto"` (measured with `ResizeObserver`; `"auto"` is the default) |
| `children` | A node, or a render prop receiving the scales |
| `aria-label` | Accessible name for the `svg` |

Look: gridlines `stroke-chart-grid`, 1px, `crispEdges`, drawn first; axis labels `fill-current` `text-[9px] tabular-nums` in `text-muted-foreground`.
Reference use (ClaudeP usage chart): height 170 plus padding `{ top: 10, right: 6, bottom: 24, left: 46 }`, tick labels in `fill-muted-foreground text-xs tabular-nums`.

Choosing a chart: tiny static single series (KPI chip, table cell) uses `TimebaseSparkline` or `TimebaseSimpleTrend`; large, live or multi-series uses ChartArea with the plot components (`TimebaseLinePlot`, `TimebaseAreaPlot`, `TimebaseStepPlot`, `TimebaseEnvelopePlot`, `TimebaseTimeAxis`, `TimebaseValueMarker`, `TimebaseChartInteractionLayer`).

Do: take series colours from `--tb-state-*` or a token; give every chart an `aria-label` and the numbers in a table or text nearby; show a `TimebaseLegendChip` for each series.
Do not: use a literal colour in a series; rely on colour alone to tell series apart (also dash or label them); draw gridlines louder than the data.

## LegendChip

`legend-chip/timebase-legend-chip.tsx`. Purpose: one legend entry. Props `label`, `swatchColor` (any CSS colour), `swatchDash`, `hideSwatch` (an eye glyph replaces the swatch), `badge`, `visible`, `onToggleVisible`, `loading`, `onClick`.
Look: `inline-flex max-w-[220px] items-center gap-1.5 px-2 py-1 rounded-md border border-divider bg-card text-[12px] font-medium`. Swatch is a 16px SVG line, 2.5px stroke. Hidden: `opacity-50` and the label struck through. The label truncates from the start (`dir="rtl"`) so the end of a dotted path stays readable; the full text is the `title`. The swatch becomes a toggle button (22px hit target, `aria-pressed`, "Hide series" or "Show series") when `onToggleVisible` is set. The badge is a neutral `TimebaseBadge` with `bg-divider text-foreground`, pinned right.

Do: one chip per series; make the toggle available when series can be hidden.
Do not: use the chip colour as the only series identifier; restyle the badge.

## ThemeSwitcher

`theme-switcher/timebase-theme-switcher.tsx`. Purpose: choose the colour scheme. A `TimebaseIconButton` (size `iconsm`, ghost) that cycles `system`, `light`, `dark`. Icon `monitor`, `sun` or `moon`; the tooltip and `aria-label` name the active mode ("System theme", "Light theme", "Dark theme"). It reads and writes through `useTheme` (the `.dark` class on `<html>` plus the `tb-theme` key in `localStorage`), so it needs no provider.

Do: place it in the topbar actions, once per app.
Do not: add a second theme control; set the theme class by hand.

## Other shared parts used by the shell and patterns

| Component | Purpose |
|---|---|
| `TimebaseCopyButton` | Copy to clipboard. `value` is a string or a getter; confirms with a `check` for 2000ms, `text-success`, polite live region; takes `label` or `aria-label`. |
| `TimebaseMenu`, `TimebaseMenuItem`, `TimebaseMenuLinkItem` | Popover menu over RAC |
| `TimebaseSelectGroup` | Segmented control (`selectionMode="single"`) or chip group (`multiple`), track `h-8` (`h-7` for `sm`) |
| `TimebaseAccordion` | Collapsible group over RAC `DisclosureGroup`, multi-open by default |
| `TimebaseDataGrid` | Domain-free table; `TimebaseDataGridTable` is a virtualised, resizable grid |
| `TimebaseErrorCard` | A full-width card that says what is wrong |
| `TimebaseInlineRow` and the detail-layout parts | Label and value rows, section labels, empty value |
| `TimebaseMeter`, `TimebaseIndeterminateBar` | Quantity gauge, indeterminate progress |
| `TimebaseRightDrawer`, `TimebaseFloatingDrawer` | Modal right drawer, non-modal dockable drawer |
| `TimebaseErrorBoundary` | `variant="page"` at the root, `variant="region"` per tab |

These are covered where the patterns use them in [07-proposals.md](07-proposals.md).
