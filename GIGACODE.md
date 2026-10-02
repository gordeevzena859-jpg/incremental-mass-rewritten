# Repository Guidelines

## Project Overview

**Incremental Mass Rewritten** is a vanilla JavaScript incremental/idle browser game inspired by Distance Incremental and Synergism. It is deployed to GitHub Pages and supports PWA offline play. The game features multiple prestige layers (Supernova → Infinity → Quantum → Darkness → etc.), challenges, ascensions, and a complex progression system.

## Project Structure & Module Organization

```
index.html              # Main game entry point (loads all JS/CSS)
hidden.html             # Secondary/hidden game page
style.css               # Main stylesheet
tooltip.css             # Tooltip-specific styles
manifest.json           # PWA manifest (offline support)

js/                     # Core game logic (loaded in order in index.html)
├── main.js             # Game loop, state, tick logic
├── saves.js            # Save/load, Decimal math extensions
├── resources.js        # Resource definitions
├── buildings.js        # Mass/tickspeed building logic
├── upgrades.js         # Upgrade system
├── challenges.js       # Challenge system
├── ranks.js            # Rank/tier progression
├── tabs.js             # Tab navigation
├── formats.js          # Number formatting
├── Element.js          # UI element helper
├── break_eternity.js   # Big number library (Decimal)
├── infinity/           # Infinity layer (core, inf, corrupted_star)
├── quantum/            # Quantum layer (entropy, primordium, chroma, qc, big_rip)
├── darkness/           # Darkness layer (dark_run, dark, matter, exotic_atoms, c16)
└── supernova/          # Supernova layer (supernova, tree, radiation, fermions, bosons)

fonts/                  # Custom fonts (TTF/woff)
images/                 # Game assets (buildings, glyphs, challenges, tree nodes, quotes)
```

**Key rule:** JS files are loaded sequentially via `<script>` tags in `index.html`. Load order matters — dependencies must come before dependents.

## Build, Test, and Development

There is **no build system**. This is a pure client-side project.

| Action | How |
|---|---|
| **Run locally** | Open `index.html` in a browser (double-click or use a local server) |
| **Test** | Open browser DevTools (F12) → Console for debugging; check localStorage for save data |
| **Deploy** | Push to the `gh-pages` branch (GitHub Pages handles deployment) |

For a better dev experience, serve via a local server:
```
python -m http.server 8000
```
Then visit `http://localhost:8000`.

## Coding Style & Naming Conventions

- **Indentation:** Tabs
- **Semicolons:** Required
- **Quotes:** Single quotes for strings
- **Variable declarations:** Mix of `var` (legacy) and `const`/`let` (newer code); prefer `const` for new code
- **Naming patterns:**
  - Game systems use **UPPER_CASE** objects: `SUPERNOVA`, `CHALS`, `RANKS`, `BUILDINGS`, `FERMIONS`
  - Helper functions use **camelCase**: `massGain()`, `enterChal()`
  - Config/constants use **UPPER_CASE**: `EINF`, `FPS`, `CONFIRMS`
  - Tab/frame IDs use numeric suffixes: `stab_frame0_0`, `tab_frame5`
- **Big numbers:** Use the `E()` / `Decimal` wrapper from `break_eternity.js`. Never use raw numbers for game values past early tiers.
- **Game state:** Mutable state lives in the global `player` object. Read-only temp data uses `tmp`.

## Testing Guidelines

There is **no automated test framework**. Testing is manual:

1. Open the game in a browser
2. Use the debug tab (if available) to manipulate game state
3. Verify mechanics in DevTools console using `player` and `tmp` objects
4. Check for console errors and visual correctness

## Commit & Pull Request Guidelines

Based on the project's git history:

- **Commit messages** are concise and descriptive:
  - `v0.7.1.6` — version bumps
  - `Patch 31.07.23` — date-based patch notes
  - `Update buildings.js` — file-specific updates
  - `Chapter 0.` — feature/content additions
- Use clear, one-line commit messages describing the change
- Include the affected file or system when relevant

## Optional: Architecture Overview

### Game Layers (progression order)
1. **Mass** → Buildings, Black Holes, Stars
2. **Supernova** → Neutron stars, Bosons, Fermions, Radiation
3. **Infinity** → Infinity Points, Theorems, Corrupted Stars
4. **Quantum** → Quantum Foam, Entropy, Primordium, Chroma
5. **Darkness** → Dark Ray, Dark Shadow, Matter tiers, Glyphs, Chapter 16

### Key Global Objects
- `player` — persistent game state (saved to localStorage)
- `tmp` — per-tick computed values (recalculated every frame)
- `E(x)` — Decimal wrapper for big number arithmetic
- `Decimal` — break_eternity.js big number class

### Save System
- Auto-saves every ~30 seconds; manual save via Settings tab
- Export/Import as base64 string or clipboard
- Offline production is supported
