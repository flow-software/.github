# 05 Iconography

Source: `components/ui/icons/timebase-icon.tsx`, `components/ui/icon-button/timebase-icon-button.tsx`, and the sizes the shared components pass.

## Rules

- Lucide only (`lucide-react`). No other icon set, no emoji, no hand-drawn SVG for a glyph Lucide has.
- Render icons through `TimebaseIcon name="..."`. It indexes a curated allow-list, `ICONS`, so every app uses the same glyph for the same meaning and a rename touches one file.
- A glyph only one app needs is not added to `ICONS`. That app imports it straight from `lucide-react` and passes it as a node where the component's `icon` prop accepts a node (`TimebaseNavButton` does). Historian imports no Lucide glyph directly today.
- Add to `ICONS` only when a shared primitive or two apps need it. Keep the list alphabetical.
- An unknown key crashes the page (the component indexes `ICONS` with no fallback). When a name arrives as data, narrow it with `isTimebaseIconName` or `toTimebaseIconName(name, fallback)` first.
- Icons inherit `currentColor`. Colour them with a token utility, never a hex.

## The wrapper

```tsx
<TimebaseIcon name="plus" size={14} />
```

| Prop | Default | Notes |
|---|---|---|
| `name` | required | A `TimebaseIconName` |
| `size` | `16` | Sets `width` and `height` in px |
| `className` | | Merged after `shrink-0` |
| rest | | Passed to the SVG. The element carries `data-icon={name}` for tests. |

The wrapper sets no `strokeWidth`, so Lucide's default (2) applies. The only place the library
changes it is `TimebaseIconButton`.

## Sizes per context

All sizes read from the shared components. Use the value for the context, do not invent a new one.

| Context | Size (px) | Source |
|---|---|---|
| Default | 16 | `TimebaseIcon` |
| Button `sm`, `iconsm` | 12 | `timebase-button.tsx` |
| Button `default`, `lg` | 14 | same |
| Button loading spinner (`refresh`, `animate-spin`) | 12 for `sm` and `iconsm`, 16 for `icon`, 14 otherwise | same |
| Icon button `icon` (36x36) | 16, stroke 1.5 | `timebase-icon-button.tsx` |
| Icon button `iconsm` (28x28) | 12, stroke 2 | same |
| Sidebar nav button | 17 | `timebase-nav-button.tsx` |
| Sidebar nav link | 18 | `timebase-nav-item.tsx` |
| Sidebar header toggle | 14 | `timebase-sidebar.tsx` |
| Sidebar edge toggle | 13 | same |
| Sidebar footer toggle | 15 | `timebase-sidebar-footer.tsx` |
| Search input glyph | 14 | `timebase-search-input.tsx` |
| Select chevron | 12 | `timebase-select.tsx` |
| Field help (`info`, `lock`) | 14 | `timebase-field-help.tsx` |
| Inline row leading icon | 14, 80% opacity | `detail-layout.tsx` |
| Legend chip eye | 14 | `timebase-legend-chip.tsx` |
| Badge leading icon | 11 | `TimebaseStatusPill` |
| Delete button trash | 13 | `timebase-delete-button.tsx` |
| Toast status glyph | 15, close glyph 10 | `timebase-toast-stack.tsx` |
| Inline notice (ClaudeP callout) | 16 in a 24px box | ClaudeP `components/Callout.tsx` |
| Error card | 18 | `timebase-error-card.tsx` |

Rule of thumb: glyph is 12 to 14 inside a control with text, 16 alone in a 36px control, 17 to 18 in navigation.

## Colour and state

| State | Class |
|---|---|
| At rest | `text-muted-foreground` |
| Hover or active | `hover:text-foreground` |
| Tone | `TONE_TEXT[tone]` (`text-success`, `text-warning`, `text-destructive`, `text-info`) |
| Disabled | Control's `disabled:opacity-50` |
| Destructive trash | `text-destructive hover:bg-destructive/10` (red at rest, 10% wash on hover) |

A tone-coloured icon never carries meaning alone. Pair it with text, or with a title and accessible name.
Decorative icons next to text get `aria-hidden`. I did not verify whether the installed `lucide-react` adds it by default, so set it.

## Name map

The registry key is the camelCase of the Lucide name unless listed here.

| Key | Lucide glyph | Meaning |
|---|---|---|
| `edit` | `Pencil` | Edit |
| `trash` | `Trash2` | Delete |
| `external` | `ExternalLink` | Opens in a new tab or leaves the app |
| `logs` | `FileText` | Logs |
| `ellipsis` | `MoreHorizontal` | Overflow menu |
| `pointer` | `MousePointerClick` | Select or click target |
| `refresh` | `RefreshCw` | Refresh, and the loading spinner |
| `sidebarCollapse` | `PanelLeftClose` | Hide the sidebar |
| `sidebarExpand` | `PanelLeftOpen` | Show the sidebar |
| `splitView` | `Columns2` | Split the workspace |
| `stop` | `Square` | Stop |
| `redo`, `undo` | `Redo2`, `Undo2` | History |
| `lineChart` | `LineChart` | Usage and trend |
| `layoutGrid` | `LayoutGrid` | Overview |
| `arrowRightLeft` | `ArrowRightLeft` | Requests, traffic |
| `x`, `xCircle` | `X`, `XCircle` | Close, error |

Meaning to glyph, so the same idea always gets the same icon:

| Meaning | Key |
|---|---|
| Add | `plus` |
| Delete or remove | `trash` |
| Edit | `edit` |
| Copy, then confirmed | `copy`, then `check` (`TimebaseCopyButton` does this) |
| Save | `save`, `saveAll` |
| Settings | `settings` |
| Search | `search` |
| Filter | `filter` |
| Refresh | `refresh` |
| Play, pause, stop | `play`, `pause`, `stop` |
| Power on or off | `power`, `powerOff` |
| Show or hide a value | `eye`, `eyeOff` |
| Information or help | `info` |
| Disabled by a rule | `lock` |
| Warning | `alertTriangle` |
| Success or verified | `check`, `badgeCheck` |
| Notifications | `bell` |
| Theme | `sun`, `moon`, `monitor` for light, dark, system |
| Disclosure | `chevronDown`, `chevronUp`, `chevronRight`, `chevronLeft` |
| Database, server, bot | `database`, `server`, `bot` |

The full list is the `ICONS` object in `timebase-icon.tsx`.

## Icon-only buttons

Use `TimebaseIconButton`. It is a ghost button by default and its `aria-label` is required by the type.

- Always an `aria-label` that names the action ("Hide sidebar", "Light theme"), not the glyph ("Trash").
- Always a tooltip. Pass `tooltip="..."` with the same words. The library does not enforce this (the prop is optional), so this spec does: an icon-only button without a tooltip fails review.
- Tooltip opens on hover and on keyboard focus. Default delay is the RAC default; icon buttons that need it faster pass `tooltipDelay`. Field help uses 150ms.
- A toggle icon button also sets `aria-pressed` (`TimebaseToggleButton`, `TimebaseSplitToggleButton`).
- A destructive glyph-only button stays ghost with `text-destructive hover:bg-destructive/10`. A destructive button that shows words is `TimebaseDeleteButton`.
- A loading icon button swaps its glyph for the spinner and keeps its size. Do not render both.
- Collapsed sidebar rows are the one allowed exception to the tooltip rule: they set `aria-label` and a native `title`.
- Minimum target is the button itself: 28px (`iconsm`) or 36px (`icon`). Do not shrink it.
