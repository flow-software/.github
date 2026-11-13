# 04 Spacing, radius, shadow, motion, z-index

Source: `styles/tokens.css` unless stated. Use the Tailwind utilities that bind to these tokens. Never a raw px, ms or cubic-bezier in app code.

## Spacing

Tailwind's 4px grid (`1` is 4px). The library sets no spacing token of its own. These are the recurring values in the shared components and in Historian. Stay on them.

| Use | Value |
|---|---|
| Between a control's icon and label | `gap-2` (8px) in buttons, `gap-2.5` (10px) in nav rows |
| Between related controls in a row | `gap-2` (8px); toolbar rows `gap-3` (12px) |
| Between icon buttons in a cluster | `gap-1` (4px) |
| Nav items in a list | `gap-0.5` (2px) |
| Card padding | `p-5` (20px) default, `p-6` (24px) large, `p-0` when the content supplies its own |
| Card header to body | `mb-4` (16px), only when there is a body |
| Dialog padding and inner gap | `p-5`, `gap-3` |
| Page padding (workspace page) | `p-7` (28px) |
| Page header to next block | `mb-4` (16px) |
| Toolbar row to the list | `mb-3` (12px) |
| Section heading to its block | `mb-2` (8px); above a section `mt-6` (24px), sub heading `mt-5` (20px) |
| Row padding in a list or table cell | `px-2.5 py-2`; catalog rows `px-5 py-2.5` |
| Empty state padding | `p-10` (40px) in a bordered list, `py-6` (24px) in a table |

### Control heights

| Control | Height | Source |
|---|---|---|
| Button `sm` | 28px (`h-7`) | `button/timebase-button.tsx` |
| Button `default` | 36px (`h-9`) | same |
| Button `lg` | 44px (`h-11`) | same |
| Icon button `icon` | 36 x 36 | same |
| Icon button `iconsm` | 28 x 28 | same |
| Select comfortable | 36px (`h-9`) | `select/select-surface.tsx` |
| Select compact | 32px (`h-8`) | same |
| Input | Not fixed: `px-3 py-2`, `text-sm leading-snug` | `input/timebase-input.tsx` |
| Switch | 36 x 20 (`w-9 h-5`), thumb 16px, 2px inset | `switch/timebase-switch.tsx` |
| Topbar, tab strip row, sidebar brand row | 56px (`h-14`) | see [01-shell.md](01-shell.md) |

Targets below 44px are common in a dense admin UI. Never go under 24px in either dimension, and give
a 28px control at least 4px of space around it.

## Radius

Base `--tb-radius-base: 0.625rem` (10px). `--tb-radius` is the working value. Utilities scale from it.

| Utility | Value | Use |
|---|---|---|
| `rounded-sm` | 6px (0.6x) | Small chips, inline code |
| `rounded-md` | 8px (0.8x) | Buttons, inputs, selects, segmented control |
| `rounded-lg` | 10px (1x) | Cards, dialogs, large button, callouts |
| `rounded-xl` | 14px (1.4x) | Large surfaces |
| `rounded-2xl` to `rounded-4xl` | 18px, 22px, 26px | Rare |
| `rounded-pill` | 9999px | Badges, status pills |
| `rounded-full` | 50% or pill | Dots, switch, avatars, round icon buttons |
| `rounded-[10px]` | 10px | Popover panel and toast, written as a literal in the library |

Rules:

- Child radius is not larger than its parent's. A control inside a card is `rounded-md` or smaller.
- Small buttons (`sm`) and nav rows use the bare `rounded` utility. Its value is Tailwind's own default, which the library does not override. I did not check it against a build. Do not add new uses of bare `rounded`.
- Pick the utility, never a px radius.

## Shadow

| Token | Value | Use |
|---|---|---|
| `shadow-xs` | `0 1px 2px 0 rgba(0,0,0,0.04)` | Selected segment |
| `shadow-sm` | `0 1px 3px 0 rgba(0,0,0,0.08), 0 1px 2px -1px rgba(0,0,0,0.04)` | Small floating handle (edge toggle) |
| `shadow-md` | `0 4px 6px -1px rgba(0,0,0,0.08), 0 2px 4px -2px rgba(0,0,0,0.04)` | Raised cards if ever needed |
| `shadow-lg` | `0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -4px rgba(0,0,0,0.05)` | Anything that floats: popover, menu, tooltip, dialog, toast, narrow-screen drawer |
| `shadow-ring` | `0 0 0 3px` of `ring` at 25% | Focus ring on inputs, buttons, nav rows |
| `shadow-{tone}-glow` | `0 0 0 3px` of the tone at 20% | Indicator dots |
| Drawer | `-24px 0 48px -12px shadow-tint` | Right-anchored drawer (`tb-drawer-shell`) |

Rules:

- Elevation is surface plus border first, shadow second. A static card has a border and no shadow.
- Only things that float carry `shadow-lg`.
- Never a custom `box-shadow`. Never a coloured shadow other than the glow and ring tokens.

## Motion

| Token | Value | Use |
|---|---|---|
| `duration-fast` | 120ms | Colour and background transitions, hover, focus, switch thumb |
| `duration-base` | 200ms | Layout changes: sidebar width, drawer slide |
| `duration-slow` | 320ms | Reserved. No shared component uses it yet. |
| `ease-standard` | `cubic-bezier(0.2, 0, 0, 1)` | Default for the two durations above |
| `ease-emphasized` | `cubic-bezier(0.2, 0, 0.1, 1)` | Reserved for larger moves |

Fixed animations in the library:

| Animation | Spec | Where |
|---|---|---|
| Dialog backdrop and panel in | `tb-fade-in`, 120ms ease-out | Confirm dialog |
| Toast in | `tb-toast-in`, 200ms ease-out, slides up 8px | Toast |
| Toast out | `tb-toast-out`, 250ms ease-in, slides down 6px | Toast |
| Toast lifetime | Exit begins at 4800ms, removed at 5200ms. Manual dismiss removes at 260ms. | `useToasts` |
| Drawer in | 200ms `cubic-bezier(0.22, 1, 0.36, 1)`, slides 24px from its edge | `tb-drawer-shell`, floating drawers |
| Tone dot pulse | `tb-pulse` 1.4s ease-in-out infinite, scale to 1.25 and opacity to 0.4 | `TimebaseToneDot pulse` |
| Live pulse | `tb-live-pulse` 1.6s infinite | `TimebaseLivePulse`, value marker |
| Indeterminate bar | `tb-indeterminate-sweep` 1.15s ease-in-out infinite | `TimebaseIndeterminateBar` |
| Copy confirm | `tb-copy-confirm` 180ms on `--tb-ease-standard`, scale pop from 0.6, motion-safe only; the tick is held 2000ms | `TimebaseCopyButton` |
| Spinner | `animate-spin` on the `refresh` icon (buttons) | Loading |
| Resize grip | Opacity only, `duration-fast`, `tb-resize-grip-x` and `-y` utilities | Dividers |

Rules:

- Animate colour, opacity and transform. Animate size only for the sidebar width.
- Pulse is for a live or transitional state, never decoration. One pulsing element in view is plenty.
- Reduced motion is honoured globally, see [08-global-styles.md](08-global-styles.md). Do not write per-component motion guards that fight it.

## Z-index ladder

Read from the shared components. Do not add a level without adding it here.

| z | Layer |
|---|---|
| 5 to 8 | Sidebar edge toggle, tab strip internals, workspace corner controls |
| 30 and 40 | ClaudeP narrow-screen sidebar backdrop and drawer (APP CHOICE) |
| 100 | Drawer shell (`tb-drawer-shell`) |
| 150 | Popovers, listboxes, tooltips |
| 200 | Toast stack, tab context menus, dialog backdrop |
| 201 | Dialog panel |

The toast stack and the dialog backdrop share 200. Mount the toast stack last in the shell root (the `overlay` slot is last) so toasts stay above a backdrop.
