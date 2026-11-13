# Flow logos

Flow Software is the company. This is its logo: the letters FLOW with a ring as the O. SVG masters
with PNG exports beside them, named `flow-<variant>-<color>.<ext>`.

| Variant | What it is |
|---|---|
| `logo-full` | The full logo, the default. PNG is 1200px wide |
| `mark` | The ring alone, for small spaces. PNG is 512px high |

There is no `wordmark`. The ring is the letter O of the name, so the letters alone read "FL W".

| Color | Fill |
|---|---|
| `color` | Letters in Charcoal `#1F1F23`, ring in the green to blue gradient |
| `white` | `#FFFFFF`, for dark backgrounds |
| `black` | `#000000` |

## Lockup with the company name

`flow-lockup-<color>.svg` and `.png` (1200px wide) put SOFTWARE under the logo, spread to the same
width as FLOW. The same three colours as above.

This is a rebuild, not the original artwork. The only existing source is the raster in
[../splash/flow-splash.png](../splash/flow-splash.png), so the layout was traced from it: SOFTWARE is
about 0.3 of the letter height, with a gap of about 0.44 of the letter height, and spans the full
width. The letters are Poppins Light turned into outlines; the raster looks like Montserrat. Replace
these files if the designer has the original.

## Source

`Infohub/dev/Flow-Server/Server/wwwroot/api/help/logo.svg`, split into letters and ring, with the
viewBox cropped (the lockup adds the outlined SOFTWARE line). Every product in that repo (Flow-Server, Flow-Config, Flow-Installer, the iOS app)
is branded Flow.
