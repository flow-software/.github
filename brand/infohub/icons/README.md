# Infohub icons

| File | What it is |
|---|---|
| `infohub-icon.svg` | Master: the ring from the logo, with the gradient |
| `favicon.ico` | 16 to 256px (11 sizes), as shipped by the Flow Server |
| `infohub-icon-<size>.png` | 16, 32, 180, 192 and 512px. Transparent, the mark centred with a 6% margin |

## Source

The `.ico` is `Infohub/dev/Flow-Server/Server/wwwroot/favicon.ico`. Its ring is a smooth gradient,
while the logo SVG colours the top segment green and the rest blue, so the two differ slightly. The SVG
master and PNGs come from the logo. The 180px PNG is transparent: iOS shows transparency as black.
