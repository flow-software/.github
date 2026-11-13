# Flow icons

| File | What it is |
|---|---|
| `flow-icon.svg` | Master: the ring from the logo, with the gradient |
| `favicon.ico` | 16 to 256px (11 sizes), as shipped by the Flow Server web app |
| `flow-icon-<size>.png` | 16, 32, 180, 192 and 512px. Transparent, the mark centred with a 6% margin |
| `flow-app.ico` | The Windows application icon of Flow Config, the installer and the data connectors (16 to 64px) |
| `flow-app-icon-ios-1024.png` | The iOS App Store icon (1024px, "Curled" corners) |

## Source

- `favicon.ico`: `Infohub/dev/Flow-Server/Server/wwwroot/favicon.ico`. Its ring is a smooth gradient,
  while the logo SVG colours the top segment green and the rest blue, so the two differ slightly.
- `flow-app.ico`: `Infohub/dev/Flow-Config/Flow.ico`.
- `flow-app-icon-ios-1024.png`: `Flow-Mobile-iOS/Flow/Assets.xcassets/AppIcon.appiconset`.
- The SVG master and the PNGs come from the logo. The 180px PNG is transparent: iOS shows
  transparency as black, so add a background if it is used as an apple-touch-icon.
