# Timebase web UI design system

One source of truth for every Timebase web UI, and for reviews of them. Part of the [design systems index](../README.md): company-wide rules will live in `flow/`, this folder is the Timebase product system. Write a UI against
it, review a UI against it, and when the two disagree, change this spec first.

## Scope

- Web apps built on React, Tailwind v4 and React Aria Components (RAC).
- Covers the shell, colour, type, spacing, icons, base components, global styles and usage rules.
- Spec only. It contains no code that apps import.

## Where the values come from

Every number and token here was read from code. Nothing is invented. Where a value is a proposal
or an app choice, the file says so.

| Rank | Source | Role |
|---|---|---|
| 1 | `Timebase.Core.React.UI` (`D:\src\flow\timebase\dev\src\Timebase.Core\src\Timebase.Core.React.UI`, package `@timebase/react-ui` 0.2.0) | The reference library. It wins any disagreement. |
| 2 | `Timebase.Historian.UI` (`...\Timebase.Historian\src\Timebase.Historian.UI`) | The template for how an app consumes the library, and for structure and tone. |
| 3 | ClaudeP vendored copy (`...\claudep\feat\RVR\remote-dashboard\src\ui\app\src\design-system`) and its shell (`...\src\shell`) | Cross-check only, plus the source of the patterns the library lacks. |

Brand rules come from [../../brand/](../../brand/README.md) (palette, [typography](../../brand/typography.md)). Where the brand kit and the library disagree the appendix says which one the spec follows and why.

Read on 2026-11-13 at Timebase commit `137b848b9` (the commit the vendored copy was taken from).
Paths in the tables below are relative to `Timebase.Core.React.UI/src` unless stated.

## Files

| File | Covers |
|---|---|
| [01-shell.md](01-shell.md) | Sidebar, topbar, page container, scroll rules, page header and action slot |
| [02-color.md](02-color.md) | Token set for light and dark, surface layering, tone recipes, contrast |
| [03-typography.md](03-typography.md) | Font families, loading, size scale, weights, headings, mono |
| [04-tokens-spacing-motion.md](04-tokens-spacing-motion.md) | Spacing, radius, shadow, motion, z-index |
| [05-iconography.md](05-iconography.md) | Lucide rules, sizes, wrapper, name map, icon-only buttons |
| [06-components.md](06-components.md) | The shared components: purpose, variants, states, do and do not |
| [07-proposals.md](07-proposals.md) | Patterns apps need that the shared set lacks, marked as proposals |
| [08-global-styles.md](08-global-styles.md) | Focus ring, selection, scrollbars, reduced motion, theme boot |
| [09-usage-rules.md](09-usage-rules.md) | Card vs section, control choice, button hierarchy, copy, accessibility |
| [10-consuming.md](10-consuming.md) | Vendored copy vs package, token lint rule, never edit the vendored copy |
| [11-review-checklist.md](11-review-checklist.md) | Pass or fail checks to run against an implementation |
| [A-appendix-differences.md](A-appendix-differences.md) | Core vs vendored copy, known gaps, things not verified |

## Conventions in this spec

| Term | Meaning |
|---|---|
| **Must** and **never** | A rule. A review fails on it. |
| **Should** | The default. Deviate only with a reason written next to the code. |
| **Proposal** | Not in the shared library yet. Do not treat it as a standard. Marked `PROPOSAL`. |
| **App choice** | A value one app picked that the library does not set. Marked `APP CHOICE`. |
| `Source:` | The file the value was read from, under `Timebase.Core.React.UI/src`. |
| Measured | A number I computed from the token values, not one the source states. |

## Rules in one screen

- Colour, radius, shadow, motion and type come from `--tb-*` tokens. Never a hex, rgb or px literal for these.
- Components come from the shared library. Apps never import `react-aria-components` directly.
- Icons are Lucide only. An icon-only button always has an `aria-label` and a tooltip.
- Light and dark are both first class. Every screen is checked in both.
- Status is never colour alone. Pair colour with text or an icon.
- The vendored copy is generated. Never edit it.
