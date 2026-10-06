# AGENTS.md

> Guidance and architectural rules for AI coding agents and human contributors working on the **Sour00001** GitHub Profile repository.

---

## 1. Project Overview

* **Repository Purpose**: Special GitHub Profile README (`Sour00001/Sour00001`).
* **Core Artifacts**:
  * [`README.md`](file:///c:/Users/Sour/Downloads/Sour00001/README.md): Public profile markup utilizing `<picture>` tags for theme switching.
  * [`banner-dark.svg`](file:///c:/Users/Sour/Downloads/Sour00001/banner-dark.svg): Dark-mode animated developer banner.
  * [`banner-light.svg`](file:///c:/Users/Sour/Downloads/Sour00001/banner-light.svg): Light-mode animated developer banner.

---

## 2. GitHub SVG Rendering & Environment Constraints

When modifying or generating SVGs for GitHub READMEs:
1. **GitHub Camo Proxy**: GitHub routes all images through a proxy cache (`camo.githubusercontent.com`).
2. **No JavaScript Allowed**: Do NOT use `<script>` tags or inline JavaScript event listeners (`onload`, `onclick`); they are stripped by GitHub's sanitizer.
3. **Self-Contained Styling**: Use embedded `<style>` blocks inside `<defs>`. All animations must use **CSS3 `@keyframes`** or native SVG `<animate>` tags.
4. **Web Fonts**: Use `@import url('...')` from Google Fonts inside `<style>`, but always include system fallbacks (`-apple-system, BlinkMacSystemFont, 'Segoe UI', monospace`).

---

## 3. Banner Architecture & Coordinate Map

* **ViewBox**: `0 0 920 370` (`width="100%" height="100%"`)
* **Layer Hierarchy** (Z-Index Order):
  1. `Background Card Canvas`: `<rect x="8" y="8" width="904" height="354" rx="10" />`
  2. `Ambient Radial Glows`: Layered gradients inside `<g clip-path="url(#card-clip)">` (`#hero-glow`, `#accent-glow`, `#teal-glow`)
  3. `Moving Grid Pattern`: `<g class="grid-anim">` translating 30×30px pattern inside `<clipPath id="card-clip">`
  4. `Terminal Window Header Bar`: `y=8` to `y=44` with macOS traffic lights (`cx=26, 42, 58`) and breadcrumb text (`~/workspace / profile.ts`)
  5. `Hero Title Text`: Centered at `x=460, y=122` driven by discrete keyframe groups (`.tf_0` – `.tf_27`)
  6. `Roles Subtitle`: Centered at `x=460, y=158`
  7. `Tech Icon Badges`: Centered horizontal row at `y=188`
  8. `Tech Pill Badges`: Centered horizontal row at `y=248`
  9. `Language Progress Bar`: Clipped at `x=36, y=296, width=848, height=10`
  10. `Language Legend`: Centered horizontal legend at `y=330`

---

## 4. Design System Tokens

| Token | Dark Mode (`banner-dark.svg`) | Light Mode (`banner-light.svg`) |
| :--- | :--- | :--- |
| **Canvas Background** | `#0d1117` | `#ffffff` |
| **Header Bar Background** | `#161b22` | `#f6f8fa` |
| **Border / Stroke** | `#30363d` | `#d0d7de` |
| **Primary Text** | `#f0f6fc` | `#1f2328` |
| **Muted Text / Breadcrumb** | `#8b949e` / `#6e7681` | `#656d76` / `#8c959f` |
| **Accent Primary (Blue)** | `#58a6ff` / `#388bfd` | `#0969da` / `#0ea5e9` |
| **Accent Glow Core** | `#00e5ff` → `#3b82f6` → `#7c3aed` | `#0ea5e9` → `#6366f1` → `#a855f7` |
| **Accent Secondary** | `#f43f5e` → `#d946ef` | `#ec4899` → `#8b5cf6` |
| **Accent Tertiary** | `#06b6d4` → `#3b82f6` | `#06b6d4` → `#3b82f6` |
| **Active Dot Status** | `#3fb950` | `#1a7f37` |

---

## 5. Typewriter Animation Engine Rules

* **Total Loop Duration**: `14.0s infinite`
* **Cadence**:
  * Typing speed: `~180ms – 250ms` per character
  * "SOUR" hold duration: `~2.5s`
  * "CURT JASPER" hold duration: `~4.5s`
  * Deleting speed: `~110ms – 140ms` per character
  * Resting cursor pause: `~2.3s`
* When modifying the title strings, update the corresponding keyframe intervals in BOTH `banner-dark.svg` and `banner-light.svg` simultaneously to avoid theme desynchronization.
