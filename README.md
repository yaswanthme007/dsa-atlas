# DSA Atlas

An interactive, single-file reference for data structures and algorithms. Every concept has a live animation you can scrub, step through, and race against other algorithms.

**29 lessons · 24 live visualizations · 0 dependencies · 715 KB total**

---

## Live Demo

Open `index.html` directly in any modern browser. No server, no build step, no internet required after first load — fonts are inlined as WOFF2/base64.

---

## Lessons

### Part 1 — Foundations

| Lesson | Visualization | Status |
|---|---|---|
| Big-O Intuition | Five growth curves racing in real time | ✓ |
| Time vs. Space | Side-by-side tradeoff explorer | ✓ |
| Arrays & Memory | Direct-address layout, pointer arithmetic | ✓ |
| Recursion & Call Stack | Call stack depth visualized per frame | ✓ |

### Part 2 — Linear Structures

| Lesson | Visualization | Status |
|---|---|---|
| Linked Lists | Singly + doubly linked, pointer animation | ✓ |
| Stacks | Push/pop with call-stack metaphor | ✓ |
| Queues & Deques | FIFO and double-ended operations | ✓ |
| Two Pointers | Two-Sum on a sorted array, O(n) scan | ✓ |

### Part 3 — Trees

| Lesson | Visualization | Status |
|---|---|---|
| Binary Trees & Traversals | Inorder/preorder/postorder, call-stack overlay | ✓ |
| Binary Search Trees | Insert 8 values + AVL right-rotation | ✓ |
| Balanced Trees | Four canonical AVL imbalance shapes | ✓ |
| Heaps | Max-heap build + heap sort extraction | ✓ |

### Part 4 — Hashing

| Lesson | Visualization | Status |
|---|---|---|
| Hash Functions | — | stub |
| Hash Tables | Separate-chaining with live probing | ✓ |
| Sets & Maps | — | stub |
| Tries | Character-by-character prefix tree | ✓ |

### Part 5 — Sorting & Searching

| Lesson | Visualization | Status |
|---|---|---|
| Linear vs. Binary Search | Same array, two strategies, O(n) vs O(log n) | ✓ |
| Comparison Sorts I — Bubble Sort | Animated bars + user-editable array + Race mode | ✓ |
| Comparison Sorts II — Merge Sort | Recursion-tree renderer showing split/merge | ✓ |
| Quick Sort | Lomuto partition, pivot highlighted | ✓ |
| Non-comparison Sorts | — | stub |

### Part 6 — Graphs

| Lesson | Visualization | Status |
|---|---|---|
| Graph Representations | Adjacency list built live next to graph | ✓ |
| BFS & DFS | Same graph, queue vs. stack highlighted | ✓ |
| Shortest Paths — Dijkstra | Weighted graph, distance table updated live | ✓ |
| Topological Sort | Kahn's algorithm, in-degree panel | ✓ |
| Minimum Spanning Trees | — | stub |

### Part 7 — Dynamic Programming

| Lesson | Visualization | Status |
|---|---|---|
| Fibonacci — Memo vs. Tab | DP grid cells filled as subproblems resolve | ✓ |
| 0/1 Knapsack | 2D DP table with dependency arrows | ✓ |
| Longest Common Subsequence | Diagonal fill with backtrack path | ✓ |

---

## Features

### Visualizations
- **FLIP animations** — every element transition starts from its live position, so scrubbing backward is always smooth
- **60fps with 50+ elements** — batch DOM reads before writes to eliminate layout thrashing
- **Click to inspect** — click any bar, tree node, or graph node to see its current value and algorithm state
- **Step captions** — every step has a plain-English explanation; `aria-live` region feeds captions to screen readers

### Controls
- **Input bar** — edit array values by hand on sorting lessons; validate, rebuild, and animate instantly
- **Randomize / Reset** — new random values or back to the original demo set
- **Race mode** — run any two of {Bubble, Insertion, Quick, Merge} sort side-by-side on the same array
- **Graph dragging** — drag any node; edges spring-follow with `cubic-bezier(0.34, 1.56, 0.64, 1)` easing

### Keyboard shortcuts
| Key | Action |
|---|---|
| `Space` | Play / Pause |
| `←` / `→` | Step back / forward |
| `J` / `K` | Previous / next lesson |
| `Ctrl+K` / `⌘K` | Command palette |
| `Esc` | Dismiss overlay / drawer |

### Accessibility & UX
- `:focus-visible` rings everywhere (keyboard-only, suppressed on mouse)
- `role="region"` and `role="group"` on canvas and transport
- Screen-reader `aria-live` caption region feeds step text
- **Light theme** — single CSS custom-property swap, persisted to `localStorage`
- **Step sounds** — Web Audio synthesized tick (off by default), three pitches for step / compare / swap
- **Print stylesheet** — hides all chrome, freezes SVG at current step, printer-friendly margins

---

## Design Notes

**Single file.** The entire app — 29 lessons, 14 renderers, all animations, all styles — lives in one `index.html`. No bundler, no framework, no runtime. Open it from a USB drive or email it as an attachment and it just works.

**WAAPI over CSS.** All transitions use the Web Animations API (`element.animate()`). This lets the FLIP system read the element's *live* transform mid-flight and animate *from* the interrupted position rather than snapping. Scrubbing backward through a sort is smooth because every new animation starts from wherever the bars actually are, not from where they were last told to go.

**Renderer contract.** Every renderer exposes exactly two methods: `mount(container)` and `renderStep(step, {duration, easing})`. The `Visualizer` timeline player knows nothing about bars, nodes, or grids — just indices and events. Adding a new data structure is 150–300 lines of renderer code with no coupling to the rest of the app.

**Layout-thrashing prevention.** `ArrayBarsRenderer.renderStep` does a single batch read of all `getComputedStyle` values before issuing any writes. With 50+ bars, the difference is one style recalculation per step vs. one per element.

**Fonts.** Three typefaces: Rajdhani (display headings), Source Serif 4 (body copy), IBM Plex Mono (code, counters, labels). Latin subsets are inlined as WOFF2/base64 — the original Google Fonts URL is the only thing that was ever external, and it's gone.

---

## Usage

```bash
# Just open it
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux

# Or serve locally if you want the URL bar
npx serve .
python3 -m http.server
```

---

## Deploying to GitHub Pages

```bash
git init
git add index.html README.md .nojekyll
git commit -m "Initial release — DSA Atlas"

# Create a new repo at github.com, then:
git remote add origin https://github.com/YOUR_USERNAME/dsa-atlas.git
git branch -M main
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: Deploy from branch → Branch: main / (root) → Save.**

Your site will be live at `https://YOUR_USERNAME.github.io/dsa-atlas/` within ~60 seconds. The `.nojekyll` file tells GitHub Pages to serve `index.html` as-is without Jekyll processing.

---

## File Size Breakdown

| Component | Size |
|---|---|
| HTML structure + CSS | ~120 KB |
| JavaScript (renderers, engine, lessons) | ~175 KB |
| Inlined WOFF2 fonts (base64, latin subset) | ~420 KB |
| **Total** | **~715 KB** |

Gzipped transfer size is roughly 200–220 KB.

---

## Inspiration

Inspired by [Rustlings](https://github.com/rust-lang/rustlings) — the idea that learning a technical subject is best done by *doing*, with immediate feedback on every step. DSA Atlas applies that to algorithms: instead of reading about how bubble sort swaps neighbors, you watch it happen and then change the input to see what changes.

---

## License

MIT — do whatever you want with it.
