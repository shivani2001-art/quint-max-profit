# Max Profit Problem

Given `n` time units, pick the mix of Theatres, Pubs, and Commercial Parks to build sequentially that maximizes total earnings from operational time. Built with vanilla JavaScript, HTML/CSS, and inline SVG.

## Demo

Open `index.html` in any modern browser. The default example `n = 13` loads on start, producing **$16,500** via the optimal mix `T: 2, P: 0, C: 0`.

## The Rules

- Only one property can be under construction at a time — builds run back-to-back.
- Once a building finishes, it earns its rate for every remaining time unit up to `n`.
- Buildings that cannot finish within `n` are dropped from consideration.

| Building | Build time | Rate / unit |
|----------|-----------:|------------:|
| Theatre  | 5  | $1,500 |
| Pub      | 4  | $1,000 |
| Commercial Park | 10 | $2,000 |

## Algorithm

### Two insights

**1. Within a chosen mix, build order is determined by rate / build_time.**

Consider building two properties A then B. A earns `rate_A * (n - buildTime_A)`, B earns `rate_B * (n - buildTime_A - buildTime_B)`. Swapping to B-then-A flips which term carries which penalty. An adjacent swap improves profit iff `rate_A / buildTime_A > rate_B / buildTime_B`.

So the optimal order sorts buildings by `rate / buildTime` descending:

| Building | rate / buildTime |
|----------|-----------------:|
| Theatre  | 300 |
| Pub      | 250 |
| Commercial Park | 200 |

Theatre first, then Pub, then Park.

**2. The space of mixes is tiny — enumerate it.**

For any `n`, the number of candidate mixes is bounded by `floor(n/5) * floor(n/4) * floor(n/10)`. Even for `n = 500` that's only ~62,500 triples, each evaluated in O(total buildings). We iterate every valid `(t, p, c)` triple with `5t + 4p + 10c ≤ n`, compute profit using the proven-optimal order, and keep all triples tied at the max.

### Why this over dynamic programming?

| Approach | Time | Space | Trade-off |
|----------|------|-------|-----------|
| **Bounded enumeration + greedy order** | O(n³ / 200) | O(1) | Simple, fast at the target scale |
| 1-D DP over time | O(n · k) | O(n) | Standard knapsack-style but needs care around build order |
| Full DP over mix | O(n³) | O(n³) | Same state space, strictly more bookkeeping |

At the scale this problem operates on (tens to low hundreds of time units), bounded enumeration is the clearest code that's still fast enough. Nothing gained by getting fancier.

## Test Cases

Reproduces the PDF spec exactly:

| n | Max earnings | Optimal mixes |
|---|-------------:|---------------|
| 7  | $3,000  | `T:1 P:0 C:0`, `T:0 P:1 C:0` |
| 8  | $4,500  | `T:1 P:0 C:0` |
| 13 | $16,500 | `T:2 P:0 C:0` |

## Features

- Numeric input for `n` with inline validation
- Max earnings callout plus list of every tied-optimal mix
- Gantt-style SVG timeline showing build and operational phases per building
- Enter key submits from the input
- Auto-runs the default example on load
- Responsive layout that reflows on mobile
- No frameworks, no build step, no dependencies
