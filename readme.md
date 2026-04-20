<div align="center">

<a href="https://quint-max-profit.vercel.app">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,20,24&height=220&section=header&text=Max%20Profit&fontSize=80&fontAlignY=38&animation=twinkling&fontColor=ffffff&desc=Schedule%20builds,%20bank%20earnings,%20maximize%20the%20timeline&descAlignY=60&descSize=17" alt="Max Profit header" width="100%">
</a>

### [→ Live demo](https://quint-max-profit.vercel.app)

<p>
  <img alt="HTML5"      src="https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white&style=for-the-badge">
  <img alt="CSS3"       src="https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white&style=for-the-badge">
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=000&style=for-the-badge">
  <img alt="SVG"        src="https://img.shields.io/badge/SVG-FFB13B?logo=svg&logoColor=white&style=for-the-badge">
  <a href="https://quint-max-profit.vercel.app"><img alt="Vercel" src="https://img.shields.io/badge/Live-Vercel-000000?logo=vercel&logoColor=white&style=for-the-badge"></a>
</p>

<p><i>Zero dependencies. Single file. Open <code>index.html</code> and go.</i></p>

</div>

---

## The Problem

Mr. X has infinite land on Mars. In `n` time units, he can build **Theatres**, **Pubs**, or **Commercial Parks** — one at a time, back-to-back. Each finished building earns its rate for every remaining time unit up to `n`. Pick the mix that maximizes total earnings.

| Building | Build time | Rate / unit |
|----------|-----------:|------------:|
| Theatre  | 5  | $1,500 |
| Pub      | 4  | $1,000 |
| Commercial Park | 10 | $2,000 |

Output format: `T: <n>, P: <n>, C: <n>` for every mix tied at the maximum.

## Quick Start

```bash
git clone https://github.com/shivani2001-art/quint-max-profit.git
cd quint-max-profit
open index.html
```

The default example `n = 13` runs on load. Enter any non-negative integer, press **Enter** or click **Calculate**, and the earnings + optimal mixes + Gantt timeline update.

## Algorithm

### Two insights

**1. Within a chosen mix, optimal build order sorts by `rate / buildTime` descending.**

Consider building A then B. A earns `rate_A · (n − buildTime_A)`, B earns `rate_B · (n − buildTime_A − buildTime_B)`. The adjacent swap argument shows A-then-B beats B-then-A iff `rate_A / buildTime_A > rate_B / buildTime_B`.

| Building | rate / buildTime |
|----------|-----------------:|
| Theatre  | **300** |
| Pub      | 250 |
| Commercial Park | 200 |

So the schedule is always: all Theatres → all Pubs → all Parks.

When multiple mixes tie, the app lists them in descending `T`, then `P`, then `C` order to match the assignment's sample output style.

**2. The mix space is tiny — enumerate it.**

For any `n`, candidate mixes are bounded by `⌊n/5⌋ · ⌊n/4⌋ · ⌊n/10⌋`. Even at `n = 500`, that's ~62,500 triples — each evaluated in O(total buildings). Iterate every valid `(t, p, c)` with `5t + 4p + 10c ≤ n`, compute profit in the proven-optimal order, and keep all triples tied at the max.

### Why this over dynamic programming?

| Approach | Time | Space | Trade-off |
|----------|------|-------|-----------|
| **Bounded enumeration + greedy order** | O(n³ / 200) | O(1) | Simple, fast at the target scale |
| 1-D DP over time | O(n · k) | O(n) | Standard knapsack-style, careful around ordering |
| Full DP over mix state | O(n³) | O(n³) | Same space traversed, strictly more bookkeeping |

At tens-to-low-hundreds time units, bounded enumeration is the clearest code that is still fast enough. Fancier doesn't help.

## Test Cases

Reproduces the spec exactly:

| n | Max earnings | Optimal mixes |
|---|-------------:|---------------|
| 4  | $0      | `T:0 P:1 C:0` |
| 7  | $3,000  | `T:1 P:0 C:0`, `T:0 P:1 C:0` |
| 8  | $4,500  | `T:1 P:0 C:0` |
| 13 | $16,500 | `T:2 P:0 C:0` |
| 49 | $324,000 | `T:9 P:1 C:0`, `T:9 P:0 C:0`, `T:8 P:2 C:0` |

## Features

- Numeric input for `n` with inline validation (non-negative integers only)
- Max earnings callout + list of every tied-optimal mix as color-coded tags
- Gantt-style SVG timeline showing each building's build phase (solid) and operational phase (striped)
- **Enter** submits from the input field
- Auto-runs the default example on load
- Responsive layout that reflows on mobile
- No frameworks, no build step, no dependencies

## Project Structure

```
.
├── index.html   # the entire app — HTML, CSS, and JS inline
└── readme.md
```

## Tech

Vanilla **HTML / CSS / JavaScript** with inline **SVG**. No bundler, no framework, no `package.json`. One file, any static host.

## License

MIT
