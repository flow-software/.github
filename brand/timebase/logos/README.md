# Timebase logos

SVG masters with PNG exports beside them, named `timebase-<variant>-<color>.<ext>`.

| Variant | What it is |
|---|---|
| `logo-full` | Mark and wordmark together, the default. PNG is 1200px wide |
| `mark` | The star alone, for small spaces. PNG is 512px high |
| `wordmark` | The name alone. PNG is 1200px wide |

| Color | Fill |
|---|---|
| `color` | Full logo and wordmark in Timebase Blue `#31AEE4`, as on the installer splash. The mark alone in Logomark Blue `#0BB6FF`, as in the app icon |
| `white` | `#FFFFFF`, for dark backgrounds |
| `black` | `#000000` |

## Source

Cut from `Timebase.Historian/src/Timebase.Historian/wwwroot/api/help/timebase-logo.svg`, which is
byte-identical to `Timebase.Core.React.UI/src/styles/swagger/timebase-logo.svg`. That file follows
the system theme (dark text in light mode, `#F2F2F2` in dark mode). The exports here use fixed fills
instead, and the viewBox is cropped to the artwork. The colour version is Timebase Blue, matching the
installer splash, which draws the star and the letters in `#31AEE4`. The standalone mark keeps the
Logomark Blue of the app icon.
