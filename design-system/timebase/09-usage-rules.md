# 09 Usage rules

How to choose and combine the parts. These rules are the defaults for every Timebase UI.
They come from conventions stated in the library's own comments and from how Historian and ClaudeP
use the parts. Where a rule is this spec's own call, it says so.

## Card, or plain section

| Use a card (`TimebaseCard`) when | Use a plain section when |
|---|---|
| The content is a self-contained unit the person scans as a whole: a summary, a panel, a form group | The content continues the page's flow: a heading followed by prose, a table or a list |
| It needs its own header and action | It needs only a heading |
| It sits in a grid with siblings of the same kind (a KPI strip) | It is the only block under its heading |
| It stands apart from the page background | The page background is already the right backdrop |

Rules:

- A page is cards or sections, not cards inside sections inside cards. One level of container.
- Never nest a card in a card. Group inside a card with a `divider` rule or spacing.
- A table or list that fills the pane may be its own bordered `bg-card` region (`rounded-md border border-border bg-card`) instead of sitting in a card.
- A card has a border and no shadow. Shadow is for things that float.
- A card's header takes at most one action. More actions go in a menu.

## Choosing a control

| The person is... | Use | Not |
|---|---|---|
| Turning a setting on or off | `TimebaseSwitch` with a state label | A checkbox, two buttons |
| Choosing 1 of 2 to 5 short options that change a view | Segmented control (`TimebaseSelectGroup`, single) | A select |
| Choosing 1 of many, or a long list | `TimebaseSelect`; `TimebaseComboBox` if it must be searchable | A segmented control, radio list |
| Choosing several of a short list | `TimebaseSelectGroup` multiple, or `TimebaseChipMultiSelect` | A multi-select dropdown |
| Entering free text | `TimebaseInput` | A select with an "other" |
| Entering a number | `TimebaseNumberField` | Text input with a pattern |
| Filtering a list | `TimebaseSearchInput` | A plain input without the glyph |
| Picking a date or time | `TimebaseDatePicker`, `TimebaseTimePicker`, `TimebaseDateTimePicker` | A text input |
| Doing something | `TimebaseButton` | A switch, a link |
| Going somewhere | `TimebaseNavItem` or a link | A button that sets `location` |

A switch takes effect as the label says. If flipping it needs a confirmation or a Save, say so next
to it, or use a button instead.

## Button hierarchy

| Level | Variant | Count |
|---|---|---|
| Primary | `primary` | One per view or dialog |
| Secondary | `outline` | As needed |
| Tertiary, toolbar, row | `ghost`, `subtle`, icon button | As needed |
| Destructive trigger | `TimebaseDeleteButton` (outline, red text) | One per record |
| Destructive confirm | `destructive`, inside the confirm dialog only | One |
| Inline link action | `link` | Rare |

Order inside a button row, left to right: Cancel (outline), then the primary action. Right-align the
row in dialogs and drawers (`justify-end gap-2`). In a toolbar, the filter is on the left and the
primary action is on the right.

Sizes: `default` for forms and page actions, `sm` in dialogs, cards, toolbars and tables, `lg` almost
never. Do not mix sizes in one row.

## Where destructive actions live

- With the other actions of the record they affect: a detail page header beside Edit and Duplicate, the editor drawer's footer rail, or a row's action group.
- A delete that shows words is `TimebaseDeleteButton`; a glyph-only delete is a ghost `TimebaseIconButton` with `text-destructive hover:bg-destructive/10`.
- Never a filled red button as the trigger. The red fill is on the confirm button inside the dialog.
- Always behind `TimebaseConfirmDialog tone="destructive"`. The flow is in [07-proposals.md](07-proposals.md).
- Hide actions a person is not allowed to use (Historian hides admin actions for viewers). Do not show a disabled destructive button with no explanation.
- Bulk destructive actions say the count in the dialog title.

## Feedback, and where it goes

| Situation | Surface |
|---|---|
| Result of an action the person took | Toast |
| Error the person must act on | Inline, next to the thing, plus a toast if the thing is off screen |
| A region failed to load | `TimebaseErrorCard`, or the app's error notice |
| A persistent condition the person should know about | Inline notice at the top of the page; attention dot on the nav item |
| Connection lost | Pass the hub state to `TimebaseAppShell`: it raises a notification on the bell and a toast on each state change. An app without a hub shows an inline error notice at the top of the page (ClaudeP). |
| A field is wrong | Text under the field, linked to it |
| Work in progress | Button `loading`, or `TimebaseIndeterminateBar` |
| Nothing to show | Empty state with the next step |

## Copy and tone

- Plain, direct, short. Say what happened or what will happen. No filler, no marketing words.
- Sentence case for titles, labels and buttons. All caps only for overlines (group labels, KPI labels).
- Buttons are a verb, or a verb and the object: "Save", "Add dataset", "Reload". The confirm button repeats the trigger's verb.
- A bare verb for delete: "Delete", "Remove" when it only detaches. The record is named in the dialog and the `aria-label`, not on the button.
- Errors say what went wrong and what to do next. No blame, no codes alone. "ClaudeP is not responding. Start it again; this page reconnects automatically."
- Empty states say why it is empty and how to fix it: "No apps yet. Create one to get a key."
- Toast titles are the outcome ("Settings saved"). One short message line at most.
- Names are the same everywhere: the sidebar label, the page title and the tab are one word. Sidebar group names are singular ("Status", "Developer").
- Numbers: abbreviate large counts (`1.2k`), keep units next to the value, use `tabular-nums` in columns.
- Plain ASCII punctuation in strings. No em dashes or en dashes. Use a period, a colon or a comma.
- Never "Click here". Never blame the person. Never "Oops".

## Accessibility minimums

A screen fails review if it breaks one of these.

| Area | Minimum |
|---|---|
| Keyboard | Everything operable with the keyboard alone, in a logical order. RAC supplies this: do not replace it with a `div` and an `onClick`. |
| Focus | A visible focus indicator on every interactive element. Focus moves into a dialog, is trapped there, and returns to the trigger on close. |
| Names | Every control has an accessible name. Icon-only: `aria-label` plus tooltip. Inputs: a label or `aria-label`. Groups and tab lists: `aria-label`. |
| Colour | Text 4.5:1, large text and meaningful non-text 3:1. Status never by colour alone: add text, an icon or a title. See [02-color.md](02-color.md) for the known gaps. |
| Status | Live status uses `role="status"` (polite); failures use `role="alert"`. The toast region is `aria-live="polite"`. |
| Errors | An invalid field is marked (`aria-invalid` or `data-invalid`), shows text, and the text is linked to the control. |
| Headings | One `h1`, no skipped levels. |
| Landmarks | `aside` with a `nav` for the sidebar, `header` for the topbar, one `main` |
| Current page | `aria-current="page"` on the active nav item |
| Motion | Honours `prefers-reduced-motion`. Nothing essential is only in an animation. |
| Targets | 24px minimum in each dimension, 28px or more for icon buttons. |
| Dialogs | `role="alertdialog"` for confirmations, a title, the destructive default focus on Cancel. |
| Dynamic text | A relative time or counter is not in a live region and does not announce on tick. |
| Automated | Run axe-core on every page and on the shell with data on screen (ClaudeP's `a11y.test.tsx` does this and includes a check that axe fails on a known bad input). |
| Zoom and reflow | The layout works at 200% zoom and at the narrow breakpoint without horizontal page scroll |
