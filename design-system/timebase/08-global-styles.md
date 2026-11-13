# 08 Global styles and theme boot

Source: `styles/tokens.css` (`@layer base`, utilities, reduced-motion block) and the boot snippet in the library README.

`tokens.css` is imported once, by the app's `index.css`:

```css
@import "tailwindcss";
@import "@timebase/react-ui/styles/tokens.css";
@source "../node_modules/@timebase/react-ui/src";
body { margin: 0; }
```

That is the whole of Historian's stylesheet. The `@source` line makes Tailwind scan the library's
sources. `tokens.css` also loads the `tailwindcss-react-aria-components` plugin, which maps RAC state
attributes to Tailwind variants (`pressed:`, `selected:`, `entering:`, `focused:`, `invalid:`, `open:`).
Dark is `@custom-variant dark (&:is(.dark *))`.

## Focus ring

| Item | Value |
|---|---|
| Token | `--tb-shadow-ring: 0 0 0 3px color-mix(in srgb, var(--tb-color-ring) 25%, transparent)` |
| Utility | `shadow-ring` |
| Colour | `ring`: `#31AEE4` light, `#4CD6F5` dark |
| Buttons, nav rows, tabs, toggles | `focus-visible:outline-none focus-visible:shadow-ring` |
| Inputs and selects | `focus:border-ring focus:shadow-ring` (select also when open) |
| Invalid field focused | Border `destructive` and a 25% destructive ring |
| Row, cell, column in a table | `outline-none focus-visible:shadow-ring` |

Rules:

- Every interactive element shows a focus indicator on keyboard focus. `focus-visible`, not `focus`, except text fields.
- Never remove the outline without putting the ring back.
- Measured: the ring at 25% is 1.23:1 against the light card and 1.79:1 against the dark card, so on a button, where the ring is the only indicator, it is below the 3:1 non-text contrast minimum. Inputs add a full-colour border (2.36:1 light). This is a known gap, see the appendix. Do not weaken the ring further, and do not hide it.

## Selection

`::selection { background: var(--tb-blue-300); color: var(--tb-neutral-900); }` in both modes (Lucky Blue with Charcoal text). Do not restyle it.

## Scrollbars

Native, themed with the standard properties only. No `::-webkit-scrollbar` rules.

```css
* {
  scrollbar-width: thin;
  scrollbar-color: var(--tb-color-scrollbar-thumb) transparent;
}
```

| Item | Value |
|---|---|
| Thumb | `--tb-color-scrollbar-thumb`: `color-mix(in srgb, var(--tb-color-foreground) 25%, transparent)`. About `#C7C7C8` on white and `#4A4A4C` on the dark page. |
| Track | `transparent`, so the gutter takes the surface behind it |
| Scope | Every element, so a `.dark` island inside a light page resolves the thumb against its own foreground |
| Opt out | A component that must hide its bar (the tab strip) sets `scrollbar-width: none` on its own element, which wins |

Rules: do not set a scrollbar colour or width per component; the thumb is a foreground tint, not a fixed grey, so it keeps contrast on any surface in both modes.

## Reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Everything animated in the library respects this without a per-component guard. The copy tick also
gates on `motion-safe:`. Do not add `!important` motion in apps, and do not rely on an animation to
convey state (a pulsing dot loses its pulse here, so its tone and label must carry the meaning).

## Other base rules

| Item | Value |
|---|---|
| `hr` | `border: 0; border-top: 1px solid var(--tb-color-border); margin: 1.5rem 0` |
| Colour scheme | `color-scheme: light` on `:root`, `dark` on `.dark` and `[data-theme="dark"]`, so native controls and scrollbars match |
| Resize grips | Utilities `tb-resize-grip-y` and `tb-resize-grip-x`: a 4 x 28px (or 28 x 4px) pill drawn with `::before`, hidden at rest, fades in over `duration-fast` on hover, focus-visible or `data-dragging` |
| Drawer shell | Utility `tb-drawer-shell`: fixed, top to bottom at the right, z 100, `bg-card`, left border, directional shadow, 200ms slide in |
| React Aria pressable rule | An app with a strict CSP moves RAC's injected `[data-react-aria-pressable]` rule into its own stylesheet (ClaudeP does, in `index.css`) |

## Theme boot

Light and dark are the `dark` class on `<html>`. The choice is the `tb-theme` key in `localStorage`
(`light`, `dark`, or absent for system). `useTheme` owns it after mount, and `TimebaseThemeSwitcher`
drives it. No provider.

To avoid a white flash, each app pastes this inline block into `<head>` of `index.html`, before any stylesheet. It is the single source of truth. Copy it verbatim.

```html
<meta name="color-scheme" content="dark light" />
<script>
  (function () {
    try {
      var stored = localStorage.getItem('tb-theme');
      var osDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
      var isDark = stored === 'dark' || (stored !== 'light' && osDark);
      document.documentElement.classList.toggle('dark', isDark);
      document.documentElement.style.backgroundColor = isDark ? '#121214' : '#F2F2F2';
    } catch (e) {
      document.documentElement.classList.add('dark');
      document.documentElement.style.backgroundColor = '#121214';
    }
  }());
</script>
```

Then, in the entry file, clear the inline background (`document.documentElement.style.backgroundColor = ''`) so CSS takes over, and follow OS changes only while no preference is stored. Historian does both in `main.tsx`. The reactive listener now also lives in `useTheme`.

Note that the boot light colour is `#F2F2F2` while the light page token is `#FFFFFF`. Visible only for the moment before the stylesheet loads.

An app whose CSP forbids inline scripts ships the same logic as a module loaded from the server (ClaudeP `src/theme/boot.ts`): read `tb-theme`, toggle `dark`, default to dark when storage is unavailable. APP CHOICE.
