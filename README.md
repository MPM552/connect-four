# Connect 4

A web-based Connect 4 game with a strong, multi-difficulty AI opponent powered by minimax search with alpha-beta pruning. The AI is the product — three difficulty levels with distinct heuristics, not randomness.

---

## Built With

| Phase | Tool |
|---|---|
| Planning | Claude Haiku 3.5+ · BMAD framework |
| Development | Claude Sonnet 4.6 |

---

## Features

- **Player vs Player** — local two-player on the same device
- **Player vs Bot** — three AI difficulty levels
  - **Easy** (depth 2) — center-column preference, no threat reading
  - **Medium** (depth 4) — horizontal/vertical threat and opportunity scoring
  - **Hard** (depth 7) — full positional evaluation across all directions with alpha-beta pruning
- Classic 7×6 board with gravity-based token placement
- Win detection in all four directions (horizontal, vertical, both diagonals)
- Draw detection when board is full
- Turn indicator, win/draw modal, new game and main menu controls

---

## Architecture

Five decoupled modules in a single `connect4.html` file:

```
GameState     board array, turn management, move history
GameRules     move validation, gravity (dropRow), win/draw detection
AIEngine      minimax + alpha-beta; swappable heuristics per difficulty
UI            board rendering, column headers, modal, turn indicator
Main          game lifecycle, screen navigation, input routing
```

**Key design decisions:**

- **Column-major board** — `board[col][row]`, row 0 = top, row 5 = bottom. Column operations are O(1).
- **Immutable moves** — `applyMove()` returns a new board copy; original state is never mutated. Enables safe AI lookahead.
- **Heuristic-pluggable AI** — all three difficulty levels share the same minimax tree. Only the scoring function differs. Swap heuristics without touching search logic.
- **Alpha-beta pruning** — reduces the branching factor significantly at depth 7, keeping hard-mode response under 2 seconds in modern browsers.

---

## Getting Started

No build step, no dependencies.

```bash
# Option 1: open directly
open connect4.html

# Option 2: serve locally
python -m http.server 8080
# then visit http://localhost:8080/connect4.html
```

**Browser support:** Chrome 90+, Firefox 88+, Safari 14+

---

## Performance Targets

| Metric | Target |
|---|---|
| Input latency | < 100ms |
| Bot response (easy/medium) | < 500ms |
| Bot response (hard) | < 2s |
| Win/draw detection | Instant (< 16ms) |
| Bundle size | < 200KB (zero dependencies) |

---

## Roadmap

**Phase 2**
- Move preview on column hover
- Game history and replay
- Win/loss statistics via localStorage
- Bot-vs-bot mode
- Keyboard input (keys 1–7)
- Token drop animation

**Phase 3** *(conditional on usage)*
- Online multiplayer
- Accounts and leaderboards
- Mobile-optimized layout
- AI self-play and reinforcement learning

---

## Project Structure

```
connect4.html   complete game (HTML + CSS + JS, self-contained)
README.md       this file
```
