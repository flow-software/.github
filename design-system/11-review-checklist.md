# 11 Review checklist

Run this against an implementation. Every item is pass or fail. The `How` column says what to read:
**Code** is a read of the source or a grep, **Shot** is a screenshot (take light and dark, at 1280px
wide and at 360px wide), **Run** needs the app running.

Record the result as a list of failing IDs, each with the file and line or the screenshot. An app passes when no item fails. Items marked `(page shell)` or `(workspace shell)` apply to that shell only. Items marked `known gap` may fail because the library itself does, see [A-appendix-differences.md](A-appendix-differences.md): flag them, do not block on them.

## Shell

| ID | Check | How |
|---|---|---|
| S1 | The root is one viewport tall (`h-screen` or `h-dvh`), `flex`, `bg-background text-foreground`. Neither the root nor `main` scrolls. | Code |
| S2 | Sidebar is 216px expanded and 58px collapsed and animates `width` over 200ms (`duration-base ease-standard`). Built from `TimebaseSidebar` and its parts, not a local copy. | Code |
| S3 | Sidebar brand row, topbar and tab strip row are all 56px (`h-14`), and their bottom borders form one unbroken line. | Code, Shot |
| S4 | A nav item is `px-2.5 py-2` expanded and `p-2.5` collapsed. The active item has `bg-accent` and the 2.5px `bg-primary` bar and `aria-current="page"`. | Code, Shot |
| S5 | Collapsed, every nav item has `aria-label` and a `title`; a badge or attention dot is still visible at the icon's corner. | Code, Run |
| S6 | Group labels are singular nouns, 10px uppercase; collapsed they become a hairline. The top group is unlabelled. | Shot |
| S7 | Support and Instance sit in the pinned region outside the scrolling nav, under one separator, Instance below Support. | Code |
| S8 | There is exactly one scroll owner per view (workspace page: the list region; document page: the single `overflow-y-auto` scroller). Wide tables scroll in their own wrapper. | Code, Run |
| S9 | Page container matches its model: workspace page `p-7` full width, or document page `mx-auto w-full max-w-* px-4 sm:px-6 pb-10 pt-4 sm:pt-5`. | Code |
| S10 | One `h1`, from `TimebasePageHeader`, with `mb-4` to the next block. | Code |
| S11 | The topbar holds only crumbs, pulse strip, search, actions, the theme switcher. No page title, no navigation. Actions are in the standard order (split toggle, theme switcher, bell, Ask Gage). | Code, Shot |
| S12 | (page shell) At 760px wide or less the sidebar is replaced by a toggled drawer: backdrop `bg-overlay`, panel expanded, closes on navigate. `Ctrl+\` toggles. | Run, Shot |
| S13 | The right side panel, if any, pushes the workspace, is at least 320px, and has a labelled resize divider. | Code, Run |

## Colour

| ID | Check | How |
|---|---|---|
| C1 | No hex, `rgb()`, `hsl()` or Tailwind palette class (`bg-red-500`) in app source, except the documented boot snippet and WebGL canvas props. The lint rule from [10-consuming.md](10-consuming.md) passes. | Code |
| C2 | No Tier 1 primitive (`--tb-neutral-*`, `--tb-blue-*`) in a component. Only semantic tokens and utilities. | Code |
| C3 | A field paints `bg-input-surface`; a dropdown, menu or tooltip paints `bg-popover`; a container paints `bg-card`; the page paints `bg-background`. | Code, Shot |
| C4 | Hover and active fills are `bg-accent`. A hovered row, a disabled field and an open dropdown are visibly distinct from their surface in dark mode. | Shot |
| C5 | No card inside a card. | Shot |
| C6 | A line inside one surface uses `divider`; the edge between two surfaces uses `border`. | Code |
| C7 | Tone styling comes from the tone records (`TONE_FILL`, `TONE_FILL_SOFT`, `TONE_DOT`, `TONE_TEXT`) or recipe tokens. No one-off `bg-*/20` for a status. | Code |
| C8 | Both themes are used and checked: every screen is readable in light and dark, with no text or border lost. | Shot |
| C9 | Body text is `foreground` or `muted-foreground`. Tone colour is not used for running text. | Code |
| C10 | Status is never colour alone: each dot, badge or tone has a word, icon or accessible name. Check with the screenshot in greyscale. | Code, Shot |
| C11 | A series colour in a chart comes from `--tb-state-*` or a token, not a literal. | Code |
| C12 | `known gap` Primary button label contrast in light mode and tone text on light cards. Flag the screen, do not fail the app. | Shot |

## Typography

| ID | Check | How |
|---|---|---|
| T1 | No `font-family` in app CSS. Only `font-sans`, `font-display`, `font-mono`. | Code |
| T2 | Fonts load from `@fontsource` (or self-hosted woff2), before `tokens.css`, with `font-display: swap`. No CDN link. | Code |
| T3 | Every font size is one of 9, 10, 10.5, 11, 12, 12.5, 13, 14, 15, 16, 24, 28px (the scale in [03-typography.md](03-typography.md)). | Code |
| T4 | Headings are `font-display`; no skipped heading level; titles are sentence case; uppercase only on overlines. | Code, Shot |
| T5 | Identifiers, keys, paths and versions are `font-mono`. Numeric columns, counters and KPI values are `tabular-nums`. | Code |

## Spacing, radius, shadow, motion

| ID | Check | How |
|---|---|---|
| K1 | Spacing uses Tailwind scale utilities. No raw `px` margin or padding. Recurring values match the table in [04-tokens-spacing-motion.md](04-tokens-spacing-motion.md). | Code |
| K2 | Radius is a utility (`rounded-md`, `rounded-lg`, `rounded-pill`). A child's radius is not larger than its parent's. | Code |
| K3 | Only floating things carry a shadow (`shadow-lg`). A static card has a border and no shadow. No custom `box-shadow`. | Code, Shot |
| K4 | Durations and easings are `duration-fast`, `duration-base`, `ease-standard`. No raw `ms` or `cubic-bezier` in components. | Code |
| K5 | Animation is limited to colour, opacity and transform (and sidebar width). At most one pulsing element in view. | Code, Run |
| K6 | z-index values are on the ladder (5, 100, 150, 200, 201). The toast stack is mounted last in the shell root. | Code |
| K7 | With the OS reduced-motion setting on, nothing animates and nothing breaks. | Run |

## Icons

| ID | Check | How |
|---|---|---|
| I1 | Icons are Lucide, rendered through `TimebaseIcon`. A glyph not in the registry is imported from `lucide-react` and passed as a node. No emoji, no inline SVG glyph. | Code |
| I2 | Icon sizes match the context table in [05-iconography.md](05-iconography.md). | Code |
| I3 | Every icon-only button is a `TimebaseIconButton` with `aria-label` **and** `tooltip`, both with the action's name. | Code |
| I4 | A delete glyph is `trash` in `text-destructive hover:bg-destructive/10`. A delete with words is `TimebaseDeleteButton`. | Code, Shot |
| I5 | Decorative icons beside text are `aria-hidden`. | Code |

## Components

| ID | Check | How |
|---|---|---|
| B1 | No import of `react-aria-components` (or `lucide-react` for a registry glyph) in app code. | Code |
| B2 | No local copy of a shared component (button, card, select, dialog, toast, tooltip, sidebar). | Code |
| B3 | At most one `primary` button per view or dialog. Row order is Cancel then primary, right aligned in dialogs. | Code, Shot |
| B4 | A destructive trigger is outline with red text, never a filled red button. The confirm dialog is `tone="destructive"`, Cancel has initial focus, the confirm label repeats the trigger's verb. | Code, Run |
| B5 | Every field has a label or `aria-label`. An invalid field shows text under it and is marked `aria-invalid` or `data-invalid`. A select has `ariaLabel` or `ariaLabelledBy`. | Code, Run |
| B6 | A switch has a state label and is used for a setting, not an action. 2 to 5 short view options use a segmented control, not a select. | Code, Shot |
| B7 | One toast stack per app, fed through `AddToastContext` and `useAddToast()`. Toasts have an outcome title and at most one message line. | Code, Run |
| B8 | A card uses `padding` `md` or `lg`, has a border, no shadow, at most one header action. | Code, Shot |
| B9 | A KPI card value is short and formatted; a tone on it is explained in the hint. | Shot |
| B10 | A badge or status dot carries words, or has an accessible name. A dot-only status has `aria-label`. | Code |
| B11 | A tooltip holds a short phrase and no interactive content. | Code |
| B12 | A chart has an `aria-label`, a legend chip per series, and its values are also available as text or a table. | Code, Shot |
| B13 | Empty states say why and what to do next. A filtered-empty list reads differently from a truly empty one. | Shot |
| B14 | Admin-only actions are hidden for people who cannot use them. | Code, Run |
| B15 | Pages are cards or sections, one level, not nested. | Shot |

## Global styles and theme

| ID | Check | How |
|---|---|---|
| G1 | `index.css` imports Tailwind then `tokens.css`, with `@source` for the library. No competing global resets or colour overrides. | Code |
| G2 | The theme boot block (or the CSP-safe module) runs before any stylesheet. `<meta name="color-scheme" content="dark light">` is present. | Code |
| G3 | The theme is `useTheme` and `TimebaseThemeSwitcher`, once, in the topbar actions. No hand-set `.dark` class. | Code |
| G4 | No custom scrollbar styling. | Code |
| G5 | Tabbing through the page shows a visible focus indicator on every interactive element. | Run, Shot |
| G6 | `known gap` The focus ring on buttons is faint (1.23:1 on the light card). Flag it, do not fail the app. | Shot |

## Copy

| ID | Check | How |
|---|---|---|
| U1 | Buttons are a verb or verb and object in sentence case. Delete reads "Delete", detach reads "Remove". | Code, Shot |
| U2 | Errors say what went wrong and what to do. No "Oops", no "Click here", no blame. | Code |
| U3 | A name is the same in the sidebar, the page title and the tab. Sidebar group names are singular. | Shot |
| U4 | No em dash or en dash in a UI string. | Code (grep) |

## Accessibility

| ID | Check | How |
|---|---|---|
| A1 | Everything works with the keyboard alone. A dialog traps focus and returns it to its trigger. | Run |
| A2 | axe-core reports zero violations on every page and on the shell with data on screen. | Run |
| A3 | Landmarks: `aside` containing `nav`, `header`, one `main`. | Code |
| A4 | Status text is in `role="status"`; failures use `role="alert"`; the toast region is `aria-live="polite"`. A ticking time is not in a live region. | Code |
| A5 | Targets are at least 24px in each dimension; icon buttons are 28px or 36px. | Code |
| A6 | At 200% zoom and at 360px width there is no horizontal page scroll and nothing is clipped. | Run, Shot |

## Consuming

| ID | Check | How |
|---|---|---|
| D1 | The app imports from one place: `@timebase/react-ui` or the `@/design-system` alias, not both. | Code |
| D2 | (vendored) `src/design-system/` matches the source at the commit recorded in its README, byte for byte. `index.ts` is the only file that differs. | Run (diff) |
| D3 | (vendored) The folder is excluded from lint and was not hand edited; the last change to it is a refresh commit. | Code |
| D4 | The token lint rule and the RAC import rule from [10-consuming.md](10-consuming.md) are in the ESLint config and pass. | Code |
