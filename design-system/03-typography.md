# 03 Typography

Source: `styles/tokens.css` (`@theme inline` font tokens) and the sizes found in the shared components.

## Families

| Token | Stack | Use |
|---|---|---|
| `--font-sans` (`font-sans`) | `"Poppins", "Inter", ui-sans-serif, system-ui, sans-serif` | Default UI text |
| `--font-display` (`font-display`) | `"Poppins", ui-sans-serif, system-ui, sans-serif` | Titles: page title, card title, dialog title, KPI value, section headings |
| `--font-mono` (`font-mono`) | `"JetBrains Mono", ui-monospace, "SF Mono", Menlo, monospace` | Code, identifiers, versions, fixed-width values |

Open point, read from code and the brand kit, not resolved here: `font-sans` lists Poppins first, so
by Tailwind v4's default (the page font follows `--font-sans`) running text renders in Poppins and Inter
is only the fallback. The brand kit (`brand/typography.md` on branch `feat/RVR/scaffold`) says Poppins
is for headings and Inter is for UI body, and that Poppins must not set body paragraphs. I did not
render a page to confirm. Until the library changes `--font-sans`, write against what the tokens say
and do not hand-set `font-family`. See the appendix.

Never set `font-family` in app CSS. Use `font-sans`, `font-display` or `font-mono`.

## Where fonts load from

The library does not load fonts. The app loads them, before `tokens.css`, so `@font-face` fires first.

| App | Mechanism | Weights |
|---|---|---|
| Historian (`src/main.tsx`) | `@fontsource/poppins`, `@fontsource/inter`, `@fontsource/jetbrains-mono` side-effect imports | Poppins 400, 500, 600, 700. Inter 400, 500, 600. JetBrains Mono 400, 500 |
| ClaudeP (`src/styles/fonts.css`) | Hand-written `@font-face` for the Latin woff2 files from the same `@fontsource` packages, because its CSP forbids inline styles and remote fonts | Inter 400, 500. Poppins 400, 500, 600, 700. JetBrains Mono 400 |

Rules:

- Self-host through `@fontsource/*`. No Google Fonts link, no CDN. The apps run on customer networks.
- `font-display: swap`.
- Load only the weights in use. Weights the components set: 400, 500, 600, 700 for Poppins; 400 and 500 for Inter at minimum; 400 and 500 for mono.

## Size scale

There is no single type ramp token. The shared components use a small set of sizes. Use one of these
and do not invent new ones.

| Size | Tailwind | Weight | Where it is used |
|---|---|---|---|
| 28px | `text-[28px]` | 600, `font-display`, `tracking-tight` | Page title (`TimebasePageHeader`) |
| 24px | `text-2xl` | 600, `font-display`, `tabular-nums` | KPI value |
| 16px | `text-base` | 500, `font-display`, `tracking-tight` | Card title; large button |
| 15px | `text-[15px]` | 600, `font-display`, `tracking-tight` | Dialog title |
| 14px | `text-sm` | 400 body, 500 for controls | Default text, button, input, nav item, option row, table cell |
| 13px | `text-[13px]` | 400, 600 for toast title | Select (comfortable), card subtitle, page description, topbar crumbs, dialog description |
| 12.5px | `text-[12.5px]` | 500 | Panel tab, compact select, version value |
| 12px | `text-xs` | 400 or 500 | Small button, tooltip, toast message, legend chip, status dot label, helper text |
| 11px | `text-[11px]` | 500 | Badge (`tracking-[0.01em]`), KPI label (uppercase, `tracking-wider`), KPI hint |
| 10.5px | `text-[10.5px]` | 400 | Option hint (uppercase, `tracking-wide`), collapsed version |
| 10px | `text-[10px]` | 600 | Sidebar group label (uppercase, `tracking-[0.1em]`) |
| 9px | `text-[9px]` | 400, `tabular-nums` | Chart axis ticks |

Page-level section headings are not in the library. ClaudeP defines them (APP CHOICE, `shell/PageFrame.tsx`):

| Level | Classes |
|---|---|
| Section heading (h2) | `font-display text-lg font-semibold`, `mt-6 mb-2` |
| Sub heading (h3) | `font-display text-base font-semibold`, `mt-5 mb-2` |

Proposal: promote these two to the library so apps stop restating them. `PROPOSAL`

## Weights

| Weight | Use |
|---|---|
| 400 | Body, descriptions |
| 500 | Controls (button, nav item, option row, panel tab), card title, badge, labels |
| 600 | Page title, dialog title, KPI value, toast title, sidebar group label |
| 700 | Loaded, not set by any shared component. Avoid. |

## Line height and tracking

| Setting | Where |
|---|---|
| `leading-tight` (1.25) | Nav rows, option rows (single-line controls) |
| `leading-snug` (1.375) | Inputs, tooltip, toast, helper text |
| `leading-relaxed` (1.625) | Dialog description, code blocks |
| Default body line height | Running text |
| `tracking-tight` | `font-display` titles |
| `tracking-wider`, `tracking-[0.1em]` | Uppercase overlines only |

Uppercase is for overlines (KPI label, sidebar group label, option hint). Never uppercase a sentence or a button.

## Mono

Use `font-mono` for a value that is an identifier or is read character by character.

| Use | Treatment |
|---|---|
| Identifiers, keys, paths, hosts | `font-mono`. `TimebaseSelect` takes `mono` to set it on the value. |
| Versions | `font-mono font-semibold text-[12.5px]` (sidebar footer) |
| Inline code | `rounded bg-muted px-1 font-mono text-xs` (ClaudeP) |
| Code block | `rounded-md bg-muted px-4 py-3.5 font-mono text-sm leading-relaxed`, `overflow-x-auto whitespace-pre` (ClaudeP) |
| Numbers in columns, counters, KPI values, chart ticks | `tabular-nums` so digits do not shift as values change |

## Heading rules

- One `h1` per page, from `TimebasePageHeader`.
- Headings are `font-display`. Body is not.
- Do not skip levels: page title `h1`, section `h2`, sub heading `h3`. Card and dialog titles are not part of the page outline unless they are real headings (the dialog title is a RAC `Heading slot="title"`).
- Titles should be sentence case: "Activity log", "Add dataset". Not all caps. The library's fixed "Instance Info" sidebar label is the one Title Case exception.
