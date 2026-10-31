# David Wang's home page

Personal home page with ongoing projects, finished projects, and a portfolio of data visualization work. It uses the **Dusk** theme, shared with the HoYo Version Tracker and the four Python chart pages: a soft gradient background (light: indigo `#eef0ff` → lilac `#f6eefc` → peach `#fff1ea`; dark: `#0f1226` → `#1d1438` → `#2a1a2e`), translucent white cards with 1px hairline borders, 16px corners and soft shadows, pill-shaped buttons and tags, and an indigo accent (`--accent`, `#5b5bd6` / `#8f8cff` dark). Fonts: Bricolage Grotesque for headings, Inter for text. The Endfield Tracker is the exception: since 2026-10-31 it has its own industrial black/gold theme (see its CLAUDE.md), and is dark only, with no light/dark switch (the one page without it). The Weather Forecast keeps the Dusk colours and cards but since 2026-10-31 replaces the gradient with a live sky of the place shown (see its CLAUDE.md). The HoYo Version Tracker likewise keeps the Dusk colours and cards over an ink-wash mountain scene that turns from day to night, with a dusk/dawn transition, when the switch is clicked (see its CLAUDE.md). David wants every project to have its own backdrop. Graph paper belongs to Paper Arcade only; its card drawing on this page keeps a grid because it pictures that project. No `backdrop-filter` on cards: blurring every card made the page slow to render.

**Light/dark switch** (added 2026-10-27): every page on the site (this page, HoYo Version Tracker, Weather Forecast, Paper Arcade and its six games, the four Python chart pages) has the same round sun/moon button in the top-right corner (`.theme-toggle`, absolutely positioned 12px from the top, so pages need at least ~48px of top padding). Pages follow the device setting until it is clicked; the choice is saved in `localStorage` under `theme` (`light`/`dark`), which all repos share because they are served from the same `mentalplatypus.github.io` origin. A tiny inline script right after the viewport meta sets `data-theme` before first paint, and another inline script after the button handles clicks, syncs other open tabs via the `storage` event, and fires a `themechange` window event. Anything drawn with `getComputedStyle` colors (canvas drawings here, in Paper Arcade and in the tracker) must redraw on `themechange`. Copy the snippet from any page when adding a new project.

Tokens `--apple`, `--pen`, `--win` and `--leaf` are chart colors for the card drawings; `--board` is the drawings' background and `--grid` their faint guide lines.

- Repository: https://github.com/mentalplatypus/mentalplatypus.github.io (public, branch **`master`**, not `main`)
- Live site: https://mentalplatypus.github.io/ (GitHub Pages redeploys on every push to `master`)

## Files

- `index.html`: the whole site in one file, with no build step. Inline CSS and JS, plus Google Fonts.
- `David-Wang-Resume.pdf`: the file behind the header's Resume button. To update the resume, replace this file under the same name with the PDF David provides, unchanged. It's public and includes his phone, email and city.
- `README.md`: visitor-facing only (live link and a one-line description). Keep editing notes here in CLAUDE.md, never in the README.

## Page layout

1. **Header**: name, one-line intro, and buttons in this order: Resume, LinkedIn, GitHub. (CodePen and Tableau Public buttons were removed on 2026-10-28 at David's request; those works are still linked from their portfolio cards.)
   - **Name hover animations** (David, 2026-10-31): hovering the name (`h1 .name`, split into one inline-block span per letter by the script at the end; the h1 keeps an `aria-label`) plays one of four animations at random, **never the same one twice in a row** (David's rule: each of the other three has a third chance), reversed when the pointer leaves. All four only move the nine letters with transforms, so they stay smooth; the name sits above the cards (`z-index: 20`) so flying letters pass over them. **Tumble**: the letters themselves break apart and bounce around the window with gravity (edges measured on each turned letter's bounding box, so they never cause sideways scroll) and settle on the bottom; every frame (offset, angle and speeds) is recorded, so leaving plays the flight backwards exactly, in at most about a second. It replaced an earlier "cubes" version (glyphs sampled into ~300 blocks on a full-window canvas) on 2026-10-31 because that stuttered. **Zip**: each letter in turn crouches and shoots up off the screen (Web Animations, 55 ms apart); leaving drops them back in turn with a squash and a small rebound. **Ghost**: an SVG turbulence/displacement filter (`#ghost`) wobbles the edges while the name fades, blurs, rises and wavers letter by letter over 1.2 s; leaving runs it back. **Flip**: the letters flip over the top one by one like a split-flap board (`rotateX` with perspective, 60 ms apart), switching to the accent colour while edge-on so they land upside-down in accent, with a small overshoot; leaving plays the same animations in reverse (delay + end delay are equal for every letter, so the flip-back staggers last letter first). Hovering again while one is reversing resumes that one instead of picking another. On touch screens a tap plays one and it reverses by itself after 2.2 s. Off entirely for reduced motion.
2. **Ongoing projects**: feature cards with `<span class="status">Active</span>`. Currently: Endfield Tracker (started 2026-10-30; canvas drawing `data-thumb="endfield"`: staggered calendar bars in the four kind colors, the later ones dashed, a content-update diamond and a red "now" line; repo `mentalplatypus/EndfieldTracker`). When the section is empty it shows a dashed `<div class="next">New projects coming soon</div>` placeholder; remove it when a project is added and put it back when the last one moves to Finished.
3. **Finished projects**, newest first, each with `<span class="status done">Finished</span>` (green): Weather Forecast (started 2026-10-27, finished 2026-10-27; canvas drawing `data-thumb="weather"`: a gradient sun at upper right partly behind a large soft-blue cloud, a small cloud at lower right and three rain dashes, modeled on a "rain with sun" icon David supplied; laid out on a 512-unit square drawn at 74% of the canvas and centered, with darker cloud and rain colors in dark mode; repo `mentalplatypus/WeatherForecast`), then HoYo Version Tracker (finished 2026-10-28; canvas drawing `data-thumb="tracker"`: three rows of version bars and a red "now" line), then Paper Arcade. Move a project here when David says it is done. Paper Arcade's card lists its six games as tags; update them if games change.
4. **Portfolio**: three groups: Dashboard · Tableau, Python · pandas & matplotlib, and Web charts · CodePen.

## Cards

- **Feature cards** (`<article class="card feature">`): a wide card with a picture on the left. Used for projects and the Tableau dashboard.
- **Grid cards** (`<article class="card">` inside `<div class="grid">`): a small card with an SVG chart drawing on top.
- Each card's main link has `class="primary"` (Live, Play, or View dashboard). It stretches over the whole card, so clicking anywhere on the card opens it. Other links (CodePen, Code, Source, Repository) sit above it and open separately. Every card needs exactly one `a.primary`.
- Only one link is underlined in red at a time on hover.

## Card drawings (`data-glyph`)

`<svg class="glyph" data-glyph="…" viewBox="0 0 150 90">` is filled in by the `GLYPHS` object in the script, using the page's theme colors.

- **Data-driven:**
  - `stack` with `data-series`: one comma-separated row per layer, bottom layer first, separated by `;`. Colors run red → yellow → green → blue (`--apple`, `--win`, `--leaf`, `--pen`), like matplotlib's Spectral palette.
  - `lines` with `data-series`: one row per line, separated by `;`. An empty value leaves a gap. Colors follow matplotlib's default order: blue, orange, green, red.
  - `pie` with `data-values`: slice sizes, e.g. `35,59,6`.
- **Fixed drawings:** `line`, `bar`, `paired`, `spine`, `bubble`, `scatter`, `box`, `choropleth`.

The four Python cards use data-driven drawings sampled from their repos' `data.csv` files: every second year for the U.S. sector chart, every tenth year from 1750 for the two CO₂ charts, and monthly for Irvine rainfall. If that data changes, resample it and update the attributes. The CodePen cards keep their fixed drawings by choice.

## Portfolio repos

Each chart lives in its own repo under `mentalplatypus` with GitHub Pages on, at `https://mentalplatypus.github.io/<repo>/`. The exception is the COVID dashboard, which is on Tableau Public. Each repo has a visitor-facing README and an About box with the same description and live link.

- **CodePen charts** (branch `main`): plain HTML/SVG/canvas, D3 or Google Charts from CDNs. `WIP-Interactive-Scatterplot-Sample-Height-Weight-Data` loads `weight-height.csv` from its own repo, because its original third-party gist was deleted.
- **Python charts** (branch `master`): `main.py` and `data.csv`, plus `chart.png` and a small `index.html` that shows it. `chart.png` was made by running the unchanged `main.py` with Python 3.10, pandas 1.4, matplotlib 3.5 and seaborn 0.12, with `plt.show()` swapped for `savefig(dpi=200, bbox_inches="tight")`. Newer matplotlib breaks these scripts (`plt.style.use("seaborn")` and the comma-joined color string). Python isn't installed on this machine; use a temporary portable copy.

## Git workflow (required)

- Commit each logical change as soon as it works, with a short imperative subject line and a body when the reason isn't obvious. Push right after.
- Run `git status` and `git diff` before committing so only intended files go in.
- Pushes go live immediately, so only push finished, working changes. Check the page in light and dark mode and at phone width (about 375px) first.
- Never force-push or amend pushed commits; revert with a new commit instead.
