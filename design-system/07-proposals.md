# 07 Patterns the shared set does not cover yet

Patterns consuming apps need. Each was checked against `Timebase.Core.React.UI` first, so this file
separates what the library already has from what is a real gap.

Nothing marked `PROPOSAL` is a standard until it lands in the library. Until then an app that needs
one builds it from the tokens and parts below, in the shape described, and does not invent a new look.

| Pattern | Status | Closest existing | Reference implementation |
|---|---|---|---|
| Collapsible section | In Core, not in the vendored set | `TimebaseAccordion`, `TimebaseGroupPanel` | ClaudeP uses native `<details>` |
| Data table | In Core, but wrong shape for short tables. `PROPOSAL` for a short-table primitive | `TimebaseDataGrid`, `TimebaseDataGridTable` | ClaudeP `components/DataTable.tsx` (RAC `Table`) |
| Key and value row | In Core, not in the vendored set | `TimebaseInlineRow` and the detail-layout parts | none needed |
| Segmented control | In Core, not in the vendored set | `TimebaseSelectGroup` single mode | ClaudeP `components/SegmentedControl.tsx` |
| Code block with copy | `PROPOSAL` for the block. Copy button is in Core. | `TimebaseCopyButton` | ClaudeP `connect/Connect.tsx` |
| Countdown and relative time | `PROPOSAL` | none | ClaudeP `shared/useNow.ts`, `shared/format.ts` |
| Empty state | `PROPOSAL` for a shared block | `TimebaseEmptyValue`, `DataGrid emptyMessage` | Historian catalog, ClaudeP `DataBody` |
| Form layout | `PROPOSAL` | `TimebasePropertyEditor` for schema-driven forms | ClaudeP `components/fields.tsx`, `settings/SettingsSection.tsx` |
| Inline notice | `PROPOSAL` | `TimebaseErrorCard`, `TimebaseCallout` (a card with title) | ClaudeP `components/Callout.tsx` |
| Destructive confirm flow | Composition of existing parts, rules only | `TimebaseDeleteButton`, `TimebaseConfirmDialog` | ClaudeP `components/ConfirmDialog.tsx` |

Where an app already has the Core component in its dependency set, use it. The "not in the vendored
set" rows are solved by vendoring the file (see [10-consuming.md](10-consuming.md)), not by a local copy.

## Collapsible section

Optional or advanced content that starts closed.

- Use `TimebaseAccordion` when there are several, or the header holds other controls. The chevron is the only toggle, so the header can hold links and buttons. Chevron `chevronRight` rotates 90 degrees when open, default size 18, panel animates `grid-template-rows` and opacity over `duration-fast`.
- A single plain disclosure may be a native `<details className="mt-2 text-sm">` with a `summary` of `cursor-pointer select-none font-medium text-muted-foreground hover:text-foreground` and the body at `pl-5 pt-1`. APP CHOICE (ClaudeP, used for "View chart data as a table").
- The label names what is inside ("Advanced settings"), not "More".
- Closed by default for content most people skip, open for content most people need.
- Keyboard: Enter and Space toggle. `aria-expanded` is set by RAC or by `<details>`.

## Data table

For short, scannable tables (tens of rows, no virtualisation). ClaudeP's table is RAC `Table`, drawn in tokens. `PROPOSAL`: promote it as `TimebaseTable`, since `TimebaseDataGridTable` is virtualised and resizable and does not suit short tables.

| Part | Spec |
|---|---|
| Wrapper | `relative overflow-x-auto`, table `w-full border-collapse text-sm` |
| Keyboard | `keyboardNavigationBehavior="tab"` when cells hold buttons or fields, so Tab reaches them. Arrow keys still move between cells. |
| Header cell | `h-9 whitespace-nowrap border-b border-border px-2.5 text-left text-xs font-medium text-muted-foreground`, sortable ones `cursor-pointer select-none hover:text-foreground` with a 12px chevron (up for ascending) that is `invisible` when unsorted so the column does not shift |
| Body cell | `border-b border-divider px-2.5 py-2 align-middle`. `divider`, not `border`, because the rule sits inside one surface. |
| Numbers | Right aligned, `tabular-nums whitespace-nowrap` |
| Secondary columns | Hidden below 640px (`hidden sm:table-cell`) on the header and its cells |
| Focus | `outline-none focus-visible:shadow-ring` on columns, rows and cells |
| Empty | One row, `px-2.5 py-6 text-center text-muted-foreground`, says what is missing and how to fix it |

For a catalog of expandable rows in a card (Historian datasets): a `rounded-md border border-border bg-card` list, rows `px-5 py-2.5` with `hover:bg-accent`, a hover-revealed edit action (`opacity-0 group-hover:opacity-100 focus-visible:opacity-100`), the list `min-h-0 overflow-auto` so only it scrolls.

Rules: sort state is visible and announced; every column has a header; row actions are icon buttons with tooltips; never colour alone for a status cell, put the word beside the dot.

## Key and value row

Use `TimebaseInlineRow` and its siblings from `detail-layout`:

| Part | Spec |
|---|---|
| `TimebaseInlineRow` | Label column fixed at 192px (`w-48`) with an optional 14px icon at 80% opacity, label `text-[13px] text-muted-foreground`; value `flex-1 min-w-0`; optional `trailing` slot at the far right (a copy icon button). Row `py-2 px-2 -mx-2 rounded-md`. |
| `TimebasePlainValue` | `text-[14px] text-foreground` |
| `TimebaseMonoValue` | `font-mono text-[13px] tabular-nums break-all` for timestamps, paths, versions |
| `TimebaseEmptyValue` | `text-[13px] italic text-muted-foreground opacity-70`, default text "Empty" |
| `TimebaseValueChips` | `flex flex-wrap gap-1.5` of neutral mono badges |
| `TimebaseStatusPill` | Success or neutral badge with a check or x, default words "Enabled" and "Disabled" |
| `TimebaseDetailSection`, `TimebaseSectionLabel` | Overline label `text-[11px] font-semibold uppercase tracking-[0.08em] text-muted-foreground`, `mb-4` |
| `TimebaseDetailDivider` | `my-7 h-px bg-border` |

Layout for a detail page: scroller `h-full overflow-y-auto`, content `p-7`, property block, divider, sections.

## Segmented control

Pick one of 2 to 5 short options that change what a view shows (Cost or Requests, a language). Exactly one is always on.

- Use `TimebaseSelectGroup selectionMode="single"` (track layout): `inline-flex h-8 gap-0.5 rounded-md border border-border bg-muted/50 p-0.5`, height `h-7` at `size="sm"`. `ariaLabel` is required.
- ClaudeP's version, for reference (APP CHOICE): track `inline-flex gap-0.5 rounded-md bg-muted p-0.5`, segment `h-6 px-2.5 text-xs font-medium text-muted-foreground`, selected `bg-card text-foreground shadow-xs`, RAC `ToggleButtonGroup` with `disallowEmptySelection`.
- Keyboard: arrow keys move, the selected segment is `aria-pressed` or selected.

## Code block with copy

`PROPOSAL`: a `TimebaseCodeBlock` that pairs the block surface with `TimebaseCopyButton`.

| Part | Spec |
|---|---|
| Block | `<pre>`, `rounded-md bg-muted px-4 py-3.5 font-mono text-sm leading-relaxed overflow-x-auto whitespace-pre`, with an `aria-label` that says what it is ("Environment variables") |
| Inline code | `rounded bg-muted px-1 font-mono text-xs` |
| Copy | A `TimebaseCopyButton` (`variant="ghost" size="sm"`) placed above the block, right aligned, or `TimebaseCopyButton` with `aria-label` as an icon in the block's corner |
| Feedback | The button's own tick for 2000ms and its polite live region. A failed copy shows an error toast: "Copy failed. Select the text and copy it manually." |
| Secrets | Show a placeholder until the person asks to reveal. Never put a real secret in the DOM unprompted. |

## Countdown and relative time

`PROPOSAL`: a `TimebaseRelativeTime` and a `useNow(intervalMs)` hook in the library.

- `useNow(ms)` returns the current time and re-renders every `ms`, so "3 min ago" stays right while nothing else changes. ClaudeP renews every 60 000 ms, which is enough for minute-level text.
- Past: under 45s "just now", under an hour "N min ago", under a day "N h ago", then a short date.
- Future: "now" at or below zero, "in N min" under an hour, "in H h M min" under a day, then weekday and time.
- Render in a `<time dateTime>` element with the absolute time in a `title`.
- Do not tick every second unless the number is the point. A per-second timer re-renders the page for no reader benefit.
- Announce nothing on tick. Do not put it in a live region.

## Empty state

`PROPOSAL`: a `TimebaseEmptyState` block. Today each app writes a centred muted line.

| Place | Spec |
|---|---|
| In a bordered list | `p-10 text-center text-sm text-muted-foreground` (Historian) |
| In a table | `px-2.5 py-6 text-center text-muted-foreground` |
| In a chart | `py-8 text-center text-sm text-muted-foreground` |
| In a value | `TimebaseEmptyValue` |

Rules: say why it is empty and what to do ("No apps yet. Create one to get a key."); a filtered-empty list says so differently ("No datasets match that filter.") from a truly empty one ("No datasets"); offer the primary action when there is one; no illustration, no tone colour.

## Form layout

`PROPOSAL`: promote ClaudeP's field rows (`TextFieldRow`, `NumberFieldRow`, `SwitchRow`, `Field`) so the closed `TimebaseInput`, `TimebaseNumberField` and `TimebaseSwitch`, which have no slot for a label, description or error, can be used in a form.

| Part | Spec |
|---|---|
| Field | `flex flex-col gap-1 text-sm`: label, control, description, error |
| Label | `text-sm font-medium text-muted-foreground`. Hide visually (`sr-only`) only when the layout already says what the field is. |
| Description | `text-xs text-muted-foreground`, linked as the control's description |
| Error | `text-xs text-destructive`, shown only while the field is invalid, linked to the control. Server errors map to the field by path. |
| Switch row | Switch and sentence on one line (`gap-2.5`), clicking the sentence toggles it. Description under it. |
| Form | One column. Related fields grouped into a section with its own heading, intro and Save button. A section's error never blocks another section. |
| Actions | `mt-3.5 flex flex-wrap items-center gap-2`: primary Save, then a status text `text-sm text-muted-foreground` with `role="status"` reading "Unsaved changes", "Saved" or empty |
| Validation | `validationBehavior="aria"`, so the browser's native bubbles are not used. Clear a field's error when it is edited. |
| Required | Say "required" in the label or description. Do not rely on an asterisk. |

For schema-driven forms use `TimebasePropertyEditor` in an editor drawer instead. In a Timebase tabbed app, editing happens in the Editor Drawer, not inline in a view.

## Inline notice

`PROPOSAL`: a `TimebaseNotice`. The library has an error card and a titled callout card but no short tone-coloured notice.

ClaudeP's `Callout`: `my-2.5 flex gap-2.5 rounded-lg border px-3.5 py-3 text-sm` with `TONE_FILL_SOFT[tone]`, a 24px icon box holding a 16px glyph in the tone colour, and the text in `text-foreground`. Error tone is `role="alert"`; others are plain.

Rules: tone colour on the box and icon, body text stays `foreground`; one to two sentences; bold only the lead phrase; an action is a small button inside the notice, not a link in prose. Use for something the person should read or act on. Use a toast for a result, an error card for a failed region.

## Destructive confirm flow

Built from existing parts. Rules, not a new component.

1. The trigger is a `TimebaseDeleteButton` (outline, red text, bare verb) in a labelled place, or a ghost icon button with `text-destructive hover:bg-destructive/10`. Never a filled red button.
2. Click opens `TimebaseConfirmDialog tone="destructive"`. Title is the question with the record named: "Delete dataset Plant1?". Description says what is lost and whether it can be undone.
3. Confirm label is the same verb as the trigger. Cancel is on the left, outline. Cancel has initial focus, so one Enter cannot destroy.
4. While the request runs: `busy`, both buttons locked, dialog not dismissible.
5. On success: close, show a success toast, return focus to a sensible place (the list, not the deleted row).
6. On failure: keep the dialog open, show the error inside it or as an error toast, leave Confirm enabled for a retry.
7. A promise-returning helper (ClaudeP `useConfirm()` resolving to a boolean through one app-level dialog) keeps call sites to one line. Host that dialog once.
8. The delete lives with the other actions of that record: a detail page header beside the labelled Edit or Duplicate buttons, an editor drawer's footer rail, or a row's action group as a glyph. The library's own comments settle that a delete showing words is `TimebaseDeleteButton` and a glyph-only one is a ghost icon button.
