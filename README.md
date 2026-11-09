# flow-software/.github

The shared home for Flow Software's GitHub organization. It holds three things:

- **The org profile**: `profile/README.md` is what everyone sees on
  [github.com/flow-software](https://github.com/flow-software).
- **The brand kit**: logos, icons, colors and type for Timebase and Infohub, under `brand/`.
- **Default community files**: contributing guide, security policy, and issue and PR templates.
  GitHub uses these for any repo in the org that does not have its own.

This repo is public, so everything in it is public. Keep internal notes, customer names and
private repo details out of it.

## Layout

```
profile/README.md          public org profile page
brand/
  README.md                how to use the brand, and the rules
  colors/                  palette as a table, JSON and CSS variables
  typography.md            typefaces, pairings and type scale
  timebase/logos/          Timebase logos
  timebase/icons/          Timebase icons
  infohub/logos/           Infohub logos
  infohub/icons/           Infohub icons
CONTRIBUTING.md            default contributing guide
SECURITY.md                default security policy
.github/ISSUE_TEMPLATE/    default issue forms
.github/pull_request_template.md
```

## Changing the brand

The values in `brand/` come from the Flow Software and Timebase brand guide, and the product UIs
use the same values in their own design tokens. When the brand changes, change it here first, then
update each product's tokens to match. The products do not read from this repo at build time.
