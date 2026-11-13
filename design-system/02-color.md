# 02 Colour

Source for every value: `styles/tokens.css`. Three tiers: primitives (raw values, never used in
components), semantic tokens (intent), and the Tailwind bindings in `@theme inline`.

Rule: components use semantic tokens through Tailwind utilities (`bg-card`, `text-muted-foreground`,
`border-border`). Never a hex, rgb or hsl literal. Never a Tier 1 primitive (`--tb-neutral-925`) in a component.

Dark mode is the `.dark` class on `<html>` (also `[data-theme="dark"]`). See [08-global-styles.md](08-global-styles.md).

## Brand and tone primitives

| Token | Value | Name | Use |
|---|---|---|---|
| `--tb-blue-500` | `#31AEE4` | Timebase Blue | Primary interactive |
| `--tb-blue-600` | `#1B81AD` | Control Blue | Hover and active for blue |
| `--tb-blue-300` | `#4CD6F5` | Lucky Blue | Info; dark mode primary hover and ring |
| `--tb-blue-logomark` | `#0BB6FF` | Logomark Blue | Brand mark only, never UI |
| `--tb-green-500` | `#36E063` | Infohub Green | Secondary call to action |
| `--tb-green-800` | `#06624E` | Jungle Green | Hover for green |
| `--tb-green-400` | `#34D399` | Lucky Green | Success |
| `--tb-orange-500` | `#F59E0B` | Vivid Orange | Warning |
| `--tb-red-500` | `#EF4444` | Coral Red | Destructive |

Blue and green are for interactive elements and highlights, never large text blocks or backgrounds
(brand kit rule). Semantic colours carry meaning and are not decoration.

## Semantic tokens: light and dark

Tailwind utility name drops the `--tb-color-` prefix (`--tb-color-card` is `bg-card`, `text-card-foreground`, `border-card`).

### Surfaces and text

| Token | Light | Dark | Purpose |
|---|---|---|---|
| `background` | `#FFFFFF` | `#121214` | The page |
| `foreground` | `#1F1F23` | `#F2F2F2` | Default text |
| `card` | `#F7F7F8` | `#262628` | Containers on the page: cards, panels, drawers, topbar, dialogs, toasts |
| `card-foreground` | `#1F1F23` | `#F2F2F2` | Text on a card |
| `popover` | `#F2F2F2` | `#2E2E31` | Anything floating above a card: dropdowns, menus, tooltips |
| `popover-foreground` | = card-foreground | = card-foreground | Text on a popover |
| `input-surface` | `#EDEDEF` | `#212123` | The fill of a form field |
| `input` | `#E5E7EB` | `#2C2C2E` | A field's border |
| `border` | `#E5E7EB` | `#2C2C2E` | The edge between two different surfaces |
| `muted` | `#E5E7EB` | `#232325` | Quiet fills: neutral badge, disabled field, segmented track |
| `muted-foreground` | `#374151` | `#A1A1AA` | Secondary text, inactive nav, icons at rest |
| `accent` | `#E5E7EB` | `#37373C` | Hover and active fill for rows, nav items, ghost buttons, focused options |
| `accent-foreground` | `#1F1F23` | `#F2F2F2` | Text on accent |
| `overlay` | `rgba(0,0,0,0.4)` | `rgba(0,0,0,0.5)` | Modal and drawer backdrop |
| `shadow-tint` | 25% of `#121214` | 45% of black | Colour of directional drawer shadows |

### Action and status

| Token | Light | Dark | Purpose |
|---|---|---|---|
| `primary` | `#31AEE4` | `#31AEE4` | Primary action, active indicator, selected state |
| `primary-foreground` | `#FFFFFF` | `#121214` | Text on primary |
| `primary-hover` | `#1B81AD` | `#4CD6F5` | Primary hover |
| `secondary` | `#36E063` | `#36E063` | Secondary call to action (rare) |
| `secondary-foreground` | `#FFFFFF` | `#121214` | |
| `secondary-hover` | `#06624E` | `#34D399` | |
| `success` | `#34D399` | `#34D399` | Success, connected, running |
| `success-foreground` | `#FFFFFF` | `#121214` | |
| `warning` | `#F59E0B` | `#F59E0B` | Warning, needs attention |
| `warning-foreground` | `#FFFFFF` | `#121214` | |
| `destructive` | `#EF4444` | `#EF4444` | Danger, failure, delete |
| `destructive-foreground` | `#FFFFFF` | `#F2F2F2` | |
| `info` | `#4CD6F5` | `#4CD6F5` | Informational |
| `info-foreground` | `#1F1F23` | `#121214` | |
| `ring` | `#31AEE4` | `#4CD6F5` | Focus ring colour |
| `switch-track` | `#82888F` | `#71717A` | Off track of a switch |
| `switch-thumb` | `#FFFFFF` | `#FFFFFF` | Thumb |

### Sidebar set

| Token | Light | Dark |
|---|---|---|
| `sidebar` | `#F7F7F8` | `#1B1B1D` |
| `sidebar-foreground` | `#1F1F23` | `#F2F2F2` |
| `sidebar-primary` | `#31AEE4` | `#31AEE4` |
| `sidebar-primary-foreground` | `#FFFFFF` | `#F2F2F2` |
| `sidebar-accent` | `#E5E7EB` | `#262628` |
| `sidebar-accent-foreground` | `#1F1F23` | `#F2F2F2` |
| `sidebar-border` | `#E5E7EB` | `#2C2C2E` |
| `sidebar-ring` | `#31AEE4` | `#4CD6F5` |

Nav items currently use `accent` and `muted-foreground`, not `sidebar-accent`. Keep it that way until the library changes.

### Lines that live inside one surface

| Token | Value | Purpose |
|---|---|---|
| `divider` | 15% of `foreground`, same recipe in both modes | A hairline or outline drawn on the same surface it sits in: legend chip edge, rule between icon buttons in a card header, table row rules in the ClaudeP table. Utilities `border-divider`, `bg-divider`. |
| `chart-grid` | 15% of `foreground` | Gridlines behind a plot. `stroke-chart-grid`, `bg-chart-grid`. Plot scaffolding, kept separate from `divider` on purpose. |
| `grid-line` | 60% of `border` | Row and column rules inside the data grid. `border-grid-line`, `divide-grid-line`. |
| `scrollbar-thumb` | 25% of `foreground` | Scrollbar thumb, see [08-global-styles.md](08-global-styles.md) |

Use `border` for the edge between two surfaces. Use `divider` for a line within one surface. `border`
is near-invisible against the surface it encloses (1.08:1 on the dark card, measured), so a rule
inside a card made with `border` vanishes.

## Surface layering

Which surface sits on which. Each step must be a real step, so no two layers share a value.

| Layer | Token | Sits on | Light | Dark |
|---|---|---|---|---|
| Page | `background` | nothing | `#FFFFFF` | `#121214` |
| Sidebar | `sidebar` | page | `#F7F7F8` | `#1B1B1D` |
| Card, panel, topbar, drawer, dialog, toast | `card` | page | `#F7F7F8` | `#262628` |
| Field | `input-surface` | card | `#EDEDEF` | `#212123` |
| Muted fill | `muted` | card | `#E5E7EB` | `#232325` |
| Popover, menu, dropdown, tooltip | `popover` | card or page | `#F2F2F2` | `#2E2E31` |
| Hover or active row | `accent` | card or popover | `#E5E7EB` | `#37373C` |

Direction of elevation:

- Light: a card is slightly darker than the white page. Fields and popovers step down again from the card, because the page is already pure white and there is no headroom above it.
- Dark: elevation is lighter. Order from low to high: page, sidebar, field, muted, card, popover, accent.

Rules:

- A field sits on a card or a popover and paints `input-surface`, never `card`. A field painted the same as its container is 1.00:1 and unreadable.
- A dropdown paints `popover`, never `card`.
- Hover fill is `accent`. It must read on both a card and a popover. Do not substitute `muted`.
- Never nest a card inside a card. Group inside a card with a `divider` rule or spacing.
- The topbar and the tab strip row follow their own surface token: `card` for the page-shell topbar, `sidebar` for the tab strip in tabbed apps (set through `stripClassName`).

## Tone recipes

`Tone` is the union `neutral | info | success | warning | error`. Domain wrappers map their own
states onto a tone first. `Source: lib/tone-classes.ts` and the recipe tokens in `styles/tokens.css`.

| Record | Recipe | Use |
|---|---|---|
| `TONE_FILL` | tinted background plus tone text plus tone border | Static identifiers: badges, status pills |
| `TONE_FILL_SOFT` | quieter background, stronger border | Selected chips, active toggles, inline notices (the ClaudeP callout) |
| `TONE_DOT` | solid tone fill plus 3px glow ring | Indicator dots |
| `TONE_TEXT` | tone text colour only | Tone-coloured text or icon inside a container that owns the fill |
| `TONE_HOVER_HINT` | 5% tone wash on hover | Off-state chips that will switch to a tone on click |

Recipe token values (`color-mix` against the named base, transparent otherwise). The same for light and dark, because the bases track the mode.

| Tone | Fill | Fill border | Quiet fill | Quiet border | Glow (3px ring) | Hover hint |
|---|---|---|---|---|---|---|
| info | 18% blue-300 | 35% blue-300 | 10% | 45% | 20% of `info` | 5% blue-300 |
| success | 16% green-400 | 35% green-400 | 10% | 45% | 20% of `success` | 5% green-400 |
| warning | 18% orange-500 | 40% orange-500 | 10% | 45% | 20% of `warning` | 5% orange-500 |
| error | 18% red-500 | 40% red-500 | 10% | 45% | 20% of `destructive` | 5% red-500 |
| neutral | `muted` fill, no border | | `muted` fill, 30% of `foreground` border | | 20% of `muted-foreground` | `muted/40` |

Tailwind names: `bg-info-fill`, `border-info-fill-border`, `bg-info-fill-quiet`, `border-info-fill-quiet-border`,
`shadow-info-glow`, `hover:bg-info-hover`. `error` is spelled `destructive` in the token names
(`bg-destructive-fill`, `shadow-destructive-glow`) and `error` in the `Tone` union.

Rule: add a new treatment by adding the recipe token to `tokens.css`, binding it in `@theme inline`,
then adding the record in `tone-classes.ts`. Never write a one-off `bg-red-500/20` in an app.

### Categorical colours

| Set | Tokens | Use |
|---|---|---|
| State chart palette | `--tb-state-1` to `--tb-state-12`: `#31AEE4 #36E063 #F59E0B #EF4444 #8B5CF6 #EC4899 #14B8A6 #84CC16 #06B6D4 #F97316 #6366F1 #EAB308` | Discrete state or enum bars and the state colour picker. Index-cycled by state order. Dark label text on them. |
| Identity accents | `identity-blue` (light `#1B81AD`, dark `#4CD6F5`), `identity-green` (light `#06624E`, dark `#34D399`), `identity-violet` `#8B5CF6`, `identity-orange` `#F97316`, `identity-pink` `#EC4899`, `identity-teal` `#14B8A6`, `identity-lime` `#84CC16` | A stable colour per identity, to tell peers apart (plugin type badges). Never state. Assign explicitly, never by position, so adding one never repaints another. |

Series colours in charts come from `--tb-state-*` or the caller. The default stroke for the trend
widgets is `var(--tb-color-primary)`.

## Contrast rules

Minimums (WCAG 2.1 AA): normal text 4.5:1, large text (18.66px bold or 24px) 3:1, icons and control
boundaries that carry meaning 3:1. Do not rely on a `border` hairline to make a shape readable.

Measured from the token values above (relative luminance, my calculation, not stated in source). Where
the source states a number it is marked `source`.

| Pair | Light | Dark |
|---|---|---|
| `foreground` on `background` | 16.43 | 16.71 |
| `foreground` on `card` | 15.34 | 13.49 |
| `muted-foreground` on `card` | 9.63 | 5.89 |
| `muted-foreground` on `popover` | 9.21 | 5.28 |
| `muted-foreground` on `input-surface` | 8.82 | 6.27 |
| `muted-foreground` on `accent` (hover row) | 8.33 | 4.62 |
| `primary-foreground` on `primary` | 2.53 (fails) | 7.40 |
| `primary-foreground` on `primary-hover` | 4.38 | not measured |
| `destructive-foreground` on `destructive` | 3.76 (large text only) | 3.36 (fails) |
| `destructive` text on `card` | 3.51 | 4.01 |
| `success` text on `card` | 1.80 (fails) | 7.86 |
| `warning` text on `card` | 2.01 (fails) | 7.03 |
| `info` text on `card` | 1.60 (fails) | 8.81 |
| `switch-track` on `card` | 3.34 | 3.13 |
| `border` on `card` | 1.16 | 1.08 |
| `card` on `background` | 1.07 | 1.24 |

What follows from it:

- Body text and labels use `foreground` or `muted-foreground`. Both pass on every surface in both modes. Never use a tone colour for body text.
- Tone colour as text (`text-success` and friends) fails in light mode on a card. Pair it with an icon and a neutral label, or put the tone text on its translucent fill and state the meaning in words. Never let colour alone carry the meaning.
- A light-mode filled primary button is white on `#31AEE4`, 2.53:1. Keep primary button labels short and on the 14px or 12px weight 500 the library sets, and do not place small primary-coloured text on light surfaces. Known gap, see the appendix.
- A line made with `border` is for separating two surfaces and is expected to be faint. A shape that must be seen (a switch track, a swatch edge, a focus ring) uses a token measured at 3:1 or more: `switch-track`, `ring`, `divider`.
- `source`: a hovered row, a disabled field and an open dropdown used to measure 1.00:1 on a dark card before the dark ladder was widened. The dark ladder in the table above is the fix. Do not collapse two of its steps.
- `source`: legend chip badge fill is `divider` with `foreground` text: 8.6:1 dark, 11.4:1 light.

## Never hard-code colour

- No hex, rgb, hsl or named colours in app CSS or class strings. Use the utilities above.
- No Tailwind palette classes (`bg-red-500`, `text-gray-600`, `bg-amber-500`). Tone and neutral tokens only.
- The one allowed exceptions are the documented ones: the FOUC boot script sets `#121214` or `#F2F2F2` on `<html>` before the stylesheet loads, and a WebGL canvas (the graph component) cannot read CSS variables so it takes raw colours through props gated on the resolved theme.
- Charts and SVG: use `stroke-chart-grid`, `fill-current`, or `var(--tb-color-*)` in `style`. Not a literal.
- A new colour need is a token request against the library, not an app-local value.
