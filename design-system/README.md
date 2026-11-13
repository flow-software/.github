# Design systems

One folder per owner, so a rule lives at the level it applies to.

| Folder | Owner | Status |
|---|---|---|
| [flow/](flow/README.md) | Flow Software, the company. Rules every product shares | Bare |
| [timebase/](timebase/README.md) | The Timebase product UIs, built on `Timebase.Core.React.UI` | Written |
| [infohub/](infohub/README.md) | The Infohub product UI | Bare |

## How the levels relate

- Brand material (logos, palette, type faces) is in [../brand/](../brand/README.md), split the same
  way: `flow/`, `timebase/`, `infohub/`.
- A product system may tighten a company rule. It may not contradict one. Where it does, the product
  system says so and why.
- Until `flow/` has content, the Timebase system stands alone. Its foundations (colour tokens, type
  scale, icon rules, shell rules) are the candidates to move up into `flow/` once a second product
  needs them.
- ClaudeP and other tools that are not a product take the Timebase system, because that is the
  library they vendor.
