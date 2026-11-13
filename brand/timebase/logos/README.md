# Timebase logos

SVG masters with PNG exports beside them, named `timebase-<variant>-<color>.<ext>`.

| Variant | What it is |
|---|---|
| `logo-full` | Mark and wordmark together, the default. PNG is 1200px wide |
| `mark` | The star alone, for small spaces. PNG is 512px high |
| `wordmark` | The name alone. PNG is 1200px wide |

| Color | Fill |
|---|---|
| `color` | Wordmark and full logo in Charcoal `#1F1F23`. The mark alone in Logomark Blue `#0BB6FF` |
| `white` | `#FFFFFF`, for dark backgrounds |
| `black` | `#000000` |

## Source

Cut from `Timebase.Historian/src/Timebase.Historian/wwwroot/api/help/timebase-logo.svg`, which is
byte-identical to `Timebase.Core.React.UI/src/styles/swagger/timebase-logo.svg`. That file follows
the system theme (dark text in light mode, `#F2F2F2` in dark mode). The exports here use fixed fills
instead, and the viewBox is cropped to the artwork. The mark in the full logo is the same shape as the
app icon. Its `color` variant is Charcoal because the source draws it that way. The mark alone is
blue, as in the app icon.
