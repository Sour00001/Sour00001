# Architecture & Technical Reference

This document provides a technical specification for the vector components, animation state machines, and styling layers used across the repository.

---

## 1. Vector Banner Anatomy

The SVG banner measures `920px × 370px` and follows a structured component hierarchy:

```text
<svg viewBox="0 0 920 370">
 ├── <defs>
 │    ├── <clipPath id="card-clip"> (Rounds background canvas to 10px radius)
 │    ├── <pattern id="grid-pattern"> (30x30px line pattern)
 │    ├── <radialGradient id="hero-glow"> (Center multi-stop radial gradient)
 │    ├── <radialGradient id="accent-glow"> (Top-right accent flare)
 │    ├── <radialGradient id="teal-glow"> (Bottom-left cyber depth)
 │    ├── <clipPath id="bar-clip"> (Rounded progress bar frame)
 │    └── <style> (CSS rules, keyframes, typography)
 │
 ├── [Layer 1] Canvas Background Card
 ├── [Layer 2] Ambient Glow Spotlights (.glow-hero, .glow-accent, .glow-teal)
 ├── [Layer 3] Moving Background Grid (.grid-anim)
 ├── [Layer 4] Terminal Window Header Bar & Traffic Lights
 ├── [Layer 5] Hero Title (Typewriter state frames .tf_0 through .tf_27)
 ├── [Layer 6] Subtitle Roles
 ├── [Layer 7] Tech Stack Icon Badges
 ├── [Layer 8] Tech Pill Badges
 └── [Layer 9] GitHub Language Distribution Bar & Legend
```

---

## 2. Animation State Machine: Typewriter (`14.0s` Loop)

The typewriter uses CSS visibility state switching on discrete SVG `<g>` groups:

| Frame Group | Rendered Text | Start Time | End Time | % Range (14.0s) |
| :--- | :--- | :--- | :--- | :--- |
| `tf_0` | `S■` | `0.00s` | `0.25s` | `0.00% – 1.78%` |
| `tf_1` | `SO■` | `0.25s` | `0.50s` | `1.79% – 3.56%` |
| `tf_2` | `SOU■` | `0.50s` | `0.75s` | `3.57% – 5.35%` |
| `tf_3` | `SOUR■` *(Hold)* | `0.75s` | `3.25s` | `5.36% – 23.20%` |
| `tf_4` – `tf_6` | `SOU■` → `SO■` → `S■` | `3.25s` | `3.70s` | `23.21% – 26.42%` |
| `tf_7` | `■` *(Pause)* | `3.70s` | `4.20s` | `26.43% – 29.99%` |
| `tf_8` – `tf_17` | `C■` → `CURT JASPE■` | `4.20s` | `6.00s` | `30.00% – 42.85%` |
| `tf_18` | `CURT JASPER■` *(Hold)* | `6.00s` | `10.50s` | `42.86% – 75.00%` |
| `tf_19` – `tf_26` | `CURT JASPE■` → `C■` | `10.50s` | `11.70s` | `75.01% – 83.56%` |
| `tf_27` | `■` *(Resting Loop Pause)*| `11.70s` | `14.00s` | `83.57% – 100.0%` |

---

## 3. Background Animations

### Seamless Moving Grid
- **Animation**: `@keyframes move-grid` (4.0s linear infinite)
- **Transform**: `translate(0px, 0px)` → `translate(-30px, -30px)`
- **Mechanism**: The pattern tiles every 30px, so moving exactly 30px along both axes creates an imperceptible, continuous looping diagonal flow.

### Ambient Breathing Spotlights
- **Hero Core Glow**: `@keyframes pulse-hero` (5.0s ease-in-out infinite alternate)
- **Top-Right Accent**: `@keyframes pulse-accent` (7.0s ease-in-out infinite alternate)
- **Bottom-Left Teal**: `@keyframes pulse-teal` (8.0s ease-in-out infinite alternate)

---

## 4. Typography Matrix

1. **Header & Title**: `'Space Grotesk'`, `'Plus Jakarta Sans'`, sans-serif (Weights: `700`, `800`)
2. **Terminal, Breadcrumb & Badges**: `'JetBrains Mono'`, monospace (Weights: `500`, `600`, `700`)
