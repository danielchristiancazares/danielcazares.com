# Changelog

## 2026-04-23 — portfolio iteration

### Hero & layout
- Hero section now has a min-height tuned so the **Selected Work** heading
  peeks ~70 px above the fold at 1440×900 (formula: `calc(93vh - 262px)` —
  holds at any viewport height).
- Added a scroll affordance at the bottom of the hero: downward chevron +
  "Selected Work" label. Fades in after 500 ms and bobs gently. Click
  smooth-scrolls to `#work-heading`. `prefers-reduced-motion` disables the
  bob via the existing global rule.

### Components
- **Status pills** — generalized `.badge` into `.status-pill[data-status]`
  with four states: `shipped` (coral), `in-progress` (amber, default),
  `research` (violet), `archived` (gray). Color + tint tokens defined
  for both themes. All three project cards now carry a status pill.
- **Project card media** — new `.card-media[data-media-type]` slot at
  the top of the card, 16:9, full-bleed via negative margins.
  Supports `image`, `svg`, `gradient`. Gradient fallback renders coral
  tint over hatch pattern with the project icon centered (no gray
  boxes). Three project PNGs compressed to WebP at 800 px / quality 85
  (22–27 KB each, down from ~1.7 MB — ~98% reduction).
- **Writing row states** — replaced the `.coming-soon` class with
  `data-state="published|coming-soon|draft"`. Published rows keep arrow
  + pointer + hover underline. Coming-soon/draft rows are 55% opacity,
  cursor default, `aria-disabled="true"`, not a link, no arrow. All
  three upcoming entries are wired to `coming-soon`.
- **Tags & chips unified** — `.tech-chips` → `.tech-list`, single source
  of truth for project tech-stack styling. `.pill` → `.chip` with
  padding/radius matching the hero `.btn`.
- **Footer** — slim ghost-pill links for Email, GitHub, and RSS (stub
  `/rss.xml`). Tagline on the opposite side. Mobile stacks left, desktop
  right-aligns via existing responsive rule.
- **Music embed** — optional `.music-embed` slot below the chips,
  collapses via `:empty` when no iframe is provided.

### Theme & meta
- Light-mode `--accent-muted` darkened from `#a0695a` to `#935e50`
  (contrast against `--paper` improved 4.00 → 4.71, passes WCAG AA for
  body text). Dark-mode tokens audited and all pass (lowest was
  `--accent-muted` at 5.88).
- Theme toggle `aria-label` now reflects target mode
  ("Switch to light/dark mode"); existing `:focus-visible` ring using
  `--accent` retained; `localStorage` + `prefers-color-scheme`
  precedence unchanged.
- Reduced light-mode hatch pattern opacity by 25% (`--pattern-color`
  alpha 0.040 → 0.030). Dark-mode hatch unchanged.
- Added `og:image`, `twitter:card=summary_large_image`, and Twitter
  title/description/image meta tags. Replaced inline-SVG favicon with
  `<link rel="icon" href="/favicon.svg">` (SVG to be supplied).

### Design showcase
- New `/design.html` page demonstrates: every status pill, card variants
  (with image / with gradient fallback / without media), writing row
  states, chips, buttons, footer links, and the theme toggle in both
  modes. `noindex` meta so it stays out of search results.

### Perf snapshot (before / after)

_Captured via `PerformanceObserver` + raw file bytes against a local
`python3 -m http.server` loaded in Chrome. Chrome for Linux is not
installed in this sandbox, so **official Lighthouse scores
(Performance / Accessibility / Best Practices / SEO) were not run**.
For those, open `http://localhost:8765/` in DevTools → Lighthouse, or
run `npx lighthouse http://localhost:8765/ --preset=desktop` on a
machine with Chrome installed._

**Byte sizes (authoritative, cache-independent):**

| Asset | Before | After | Δ |
|---|---|---|---|
| `index.html` | 37.4 KB | 40.6 KB | +3.3 KB (+8.7%) |
| `spacecraft.webp` | — | 21.8 KB | new |
| `blackhole.webp` | — | 26.9 KB | new |
| `healthcare.webp` | — | 24.7 KB | new |
| **Total served** | 37.4 KB | 114.0 KB | +76.7 KB |
| **Initial viewport (with lazy-loading)** | 37.4 KB | ~62.4 KB | +25 KB |
| DOM elements | 122 | 145 | +23 |

Source PNGs (`spacecraft.png`, `blackhole.png`, `healthcare.png`) total
~5.2 MB on disk; the WebPs are a **~98.5% reduction** at visually
indistinguishable quality (85). Two of the three card images are below
the fold and carry `loading="lazy"` so they don't count against initial
load.

**Timing metrics (indicative only — Google Fonts caching between runs
skews the comparison; treat as "not regressed", not as absolute
numbers):**

| Metric | Baseline (cold) | After (warm, partial cache) |
|---|---|---|
| Time to First Byte | 5 ms | 5 ms |
| First Contentful Paint | 488 ms | 60 ms* |
| Largest Contentful Paint | 540 ms (`H1.wordmark`) | 60 ms (`P.lead`)* |
| DOMContentLoaded | 461 ms | 14 ms* |
| Load event | 684 ms | 31 ms* |

<sub>*warm-cache artifact; Google Fonts were cached between runs.</sub>
