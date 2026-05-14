# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Japanese prefecture memorization quiz app (**都道府県おぼえよう**). The existing file `index-todofuken.html` serves as a template/reference for building new web apps in the same single-file architecture.

## Architecture

**Single-file HTML apps** — all HTML, CSS, and JavaScript live in one `.html` file. Vue 3 is loaded from CDN (no build step, no npm).

```
index-todofuken.html  ← reference app (prefecture quiz)
index.html            ← new app being built (create here)
```

### File structure within each HTML file

1. `<style>` block — all CSS (mobile-first, 375px fixed width)
2. `<div id="app">` — Vue template
3. `<script>` block — Vue 3 app (`Vue.createApp({...}).mount('#app')`)

### Data persistence

IndexedDB via two helpers copied from the reference app:
```js
async function dbGet(key) { ... }
async function dbSet(key, value) { ... }
```

### Vue 3 usage pattern

Uses the **global build** (Options API or Composition API both work):
```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

App is mounted with `Vue.createApp({ data(), computed, methods, async mounted() }).mount('#app')`.

## Running / Development

No build system. Open the HTML file directly in a browser (or use VS Code Live Server). Refresh to see changes.

## UI Conventions from Reference App

- Fixed width `375px`, centered — mobile simulator layout
- Primary color: `#1a56db` (blue)
- Correct feedback: `#22c55e` (green), Wrong: `#ef4444` (red)
- CSS animations for XP floats, level-up bounces, flash feedback
- Progress dots row showing per-question results within a session
- Modal overlays for results/level-up screens

## Key Patterns to Reuse

- **Weighted random selection** (`buildSession()`) — prioritize items the user gets wrong
- **XP + leveling system** — 100 XP per level, combo bonuses for streaks
- **Per-item stats** stored in `profile.perItemStats[id] = { asked, wrong, lastSeenTurn }`
- **Session model** — fixed number of questions (e.g., 10) per round, then show results
