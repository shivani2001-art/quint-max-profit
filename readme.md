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

The app uses dynamic programming over elapsed time.

For each time `t`, `dp[t]` stores the best profit that can still be earned from `t` until `n`, plus every building-count mix that achieves that profit.

At each state, try building one more:

```txt
finish = currentTime + buildTime
profit = buildingRate * (n - finish) + dp[finish]
```

A building is only valid if `finish < n`, because a building that finishes exactly at `n` has no operational time left and earns nothing.

Since there are only three establishment types, each DP state checks only three choices:

| Building | Build time | Rate / unit |
|----------|-----------:|------------:|
| Theatre  | 5  | $1,500 |
| Pub      | 4  | $1,000 |
| Commercial Park | 10 | $2,000 |

When multiple choices produce the same max profit, the app keeps all tied mixes, removes duplicates, and lists them in descending `T`, then `P`, then `C` order.

### Why DP now?

| Approach | Time | Space | Trade-off |
|----------|------|-------|-----------|
| Brute-force count enumeration | O(n³) | O(1) | Very simple for exactly 3 types |
| **1-D DP over elapsed time** | **O(n · k)** | **O(n)** | Faster and scales cleanly |

`k = 3` here, so the DP is effectively linear in `n`.

The earlier brute-force version was chosen because the PDF gives only three fixed building types, making all count combinations easy to try, correct to reason about, and simple to verify. DP is the cleaner optimized version because the decision is based on elapsed time: from each time state, choose the next building that gives the best future profit.

## Test Cases

Validated against the assignment examples and reviewer clarification:

| n | Max earnings | Optimal mixes |
|---|-------------:|---------------|
| 7  | $3,000  | `T:1 P:0 C:0`, `T:0 P:1 C:0` |
| 8  | $4,500  | `T:1 P:0 C:0` |
| 13 | $16,500 | `T:2 P:0 C:0` |
| 49 | $324,000 | `T:9 P:0 C:0`, `T:8 P:2 C:0` |

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
