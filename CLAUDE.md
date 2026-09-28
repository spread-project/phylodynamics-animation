# CLAUDE.md

Guidance for Claude (and other contributors) working on this repository.

## What this is

`index.html` is a single-file, dependency-free animated explainer of phylodynamics. Its nine steps are listed in `README.md`. The audience is scientifically literate but not necessarily specialist, so explanations must stay accurate without heavy jargon.

## Hard constraints

- **Keep it a single self-contained file.** Everything lives in `index.html`: no build step, no bundler, no npm, no external JS libraries. The only external resource is Google Fonts, and every font has a system fallback stack.
- **No network calls or browser storage** are needed; don't add them.
- **Stay accessible.** Every change must keep working on mobile (about 360 px wide), in both light and dark themes, and with `prefers-reduced-motion`, where each step renders at its final frame.
- **Keep the science correct.** Parameter definitions, formulas and tool descriptions must match the literature. When adding a claim about a method or tool, cite it in the scene caption and/or the `.refs` paragraph, and in `README.md`.

## Architecture (inside the `<script>`)

- **Helpers**
  - `rng(seed)` is a seeded PRNG (mulberry32). Use it everywhere so animations are reproducible; never call `Math.random`.
  - `expRand`, `gauss`, `seg(t,a,b)` (clamped 0–1 progress), `ease` and `lerp` cover sampling and timing.
  - `line`, `circ`, `rect` and `text` return SVG strings.
- **Colors must be CSS variables applied via `style`,** because SVG presentation attributes don't resolve `var()`. The helpers already handle this. The theme tokens are `--bg`, `--surface`, `--ink`, `--muted`, `--line`, the nucleotide palette `--A`, `--C`, `--G`, `--T`, and the extra color `--P`.
- **Simulators**
  - `simBD(opts)` is a Gillespie birth–death simulator. It supports optional migration between two types (`mig`) and a single rate change (`tChange`, `lamAfter`), and returns `null` if the tree exceeds `max` branches.
  - `prepTree(sim)` marks which branches have sampled descendants (`kept`) and computes full (`yf`) and pruned (`yp`) layouts.
  - `findTree(params, ok, seed0, tries)` searches seeds until `ok(sim)` passes, so fixed-seed trees always look reasonable.
  - `drawTree(sim, opts)` renders a tree clipped at `tNow`. It can blend between the full and pruned layouts (`mix`), fade unsampled lineages (`fade`), and color branches by type.
  - `simCoal` and `drawCoal` cover the coalescent step.
- **Scenes**
  - Each scene is an object `{ dur, render(t) }`. `render(t)` returns SVG inner markup for a `0 0 400 300` viewBox, given the seconds elapsed since the scene started. Scenes are stateless per frame; all animation is a function of `t`.
- **Scene list**
  - The `scenes` array holds `{ s, title, caption, bd? }`, and its order is the step order.
  - `bd: true` shows the birth–death control panel (`#bdPanel`). That panel's state lives in `S5state`, and `runBD()` regenerates its tree.
- **Player**
  - `go(i)` switches scenes and updates the caption and the step bar.
  - `frame()` re-renders each animation frame until `t > dur + 0.3`, then stops (`finalDone`) to save CPU.

## Common tasks

- **Add a step:**
  1. Write a scene object with `dur` and `render(t)`.
  2. Insert it into `scenes` at the right position.
  3. Update `grid-template-columns: repeat(N,1fr)` in `.steps` and the step count in the header text ("Nine short animations").
  4. Update the step list in `README.md`.
- **Reorder steps:** reorder the `scenes` array only. The scene variable names (`S1`…`S8`, `S7b`) are just identifiers and don't need renaming.
- **Change the look:** edit the CSS tokens on `:root`. Keep the light theme, the dark theme and the `[data-theme="dark"]` block in sync.

## Testing

There is no test framework. After any change:

1. Open `index.html` in a browser and step through every scene at desktop and mobile widths, in both color schemes.
2. Smoke-test the rendering in Node to catch `NaN` or `undefined` in the generated SVG. Extract the `<script>` contents, stub `document`, `window`, `performance` and `requestAnimationFrame`, expose `scenes`, and call `scene.s.render(t)` for several values of `t`. Every call should return a non-empty string containing no `NaN` or `undefined`.

## Licensing

The code is MIT (`LICENSE`); the text and visual content is CC BY-NC 4.0 (`LICENSE-CONTENT.md`). Keep the license notice at the bottom of `index.html`, and don't add third-party code or text under an incompatible license.

## Style of copy

Write captions in plain, active sentences of three to six per step, in sentence case. Explain what a parameter *means* before naming it, and avoid hype. Keep in-SVG labels short (about 60 characters at 10–12 px on the 400-wide viewBox).
