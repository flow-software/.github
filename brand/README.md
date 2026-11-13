# Brand kit

Logos, icons, colors and type for Flow Software, Timebase and Infohub. Take assets from here
rather than from a product's source tree, so everyone uses the same files. The logos and icons were
cut from the product source trees; each folder's README says which file.

## What is where

| Folder | Holds |
|---|---|
| `colors/` | The palette: [palette.md](colors/palette.md) to read, `tokens.json` and `tokens.css` to use in code |
| [typography.md](typography.md) | Typefaces, approved pairings and the type scale |
| `timebase/logos/`, `infohub/logos/` | Logos per product |
| `timebase/icons/`, `infohub/icons/` | App icons and favicons per product |

## Files and formats

- SVG is the master for every logo and icon. Add PNG exports next to it at the sizes people ask for.
- Name files `<product>-<variant>-<color>.<ext>`, for example `timebase-logo-full-color.svg` or
  `infohub-mark-white.png`.
- Keep design source files (Figma, Illustrator) in the design tool and link to them. Only exported
  files go here.
- Commit font files only if their license allows redistribution. This repo is public.

## Rules

Taken from the brand guide:

- Timebase Blue and Infohub Green are for interactive elements and highlights only, never for large
  text blocks or backgrounds.
- Use the neutrals (Charcoal, Anti-Flash White, Iron Gray, Light Gray) for structure: backgrounds,
  text and containers.
- Semantic colors carry meaning: blue for interactive, red for danger, orange for warning, green for
  success. Use them for that meaning, not for decoration.
- Logomark Blue `#0BB6FF` is for the brand mark only, never for UI.
- In dark mode, reduce saturation on bright chips and badges and flip their text to dark. Do not
  just invert everything.
