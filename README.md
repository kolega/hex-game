# Hex Game — dots and chain splits

## About the game
A small strategy/puzzle prototype played on a single big hexagon made of many small hex tiles (side length 5). Each tile can contain up to three dots. On the fourth increment the tile “splits” and sends +1 to three random neighbors, while itself resets to one dot. This can trigger chain reactions.

Origin story: back in the early 2000s I saw a demo of a similar hex-based arcade game but forgot its name. I tried to find it years later but couldn’t, so I decided to write a tiny analogue myself — a few evenings of just-for-fun coding.

## Rules
- **Board**: one hex made of small hexagons (side length 5).
- **Dot add**: clicking a hex adds +1 dot (up to 3).
- **Split at 4**:
  - On the 4th click the hex “splits”: three random neighboring hexes each receive +1; the original hex becomes 1.
  - The parent hex is excluded from being selected as one of the three neighbors in that split step.
- **Cascade**:
  - If a neighbor already has 3 dots and receives +1, it also splits (chain reaction).
  - Chain depth is limited to 3 cycles per action.
- **Two players**:
  - Player (blue) and Enemy (red) take turns.
  - Ownership (color) of a hex is set by splits. A simple +1 on someone else’s hex does not recolor it.
  - On the player’s turn, using buffs does not end the turn; the turn ends after the player clicks a hex (unless a split occurred — see below).
  - If your action caused a split, you keep the turn (you can act again). If no split occurred, the turn passes to the opponent.
- **Buffs**:
  - Earned from captures during a single action:
    - Capture 2 enemy hexes → +1 Bomb.
    - Capture 3 or more enemy hexes → +1 Big Bomb.
  - Effects:
    - Bomb: reset one target hex to empty (any color).
    - Big Bomb: reset target hex to empty and -1 to all its neighbors (empty neighbors lose ownership).
  - Player: applies one buff per click (buff use does not end turn).
  - Enemy: may apply several buffs in a row in its turn with some probability (then performs a normal move).
- **Win conditions**:
  - All hexes have exactly 3 dots (fills the board completely), or
  - Territory capture: all hexes on the board are occupied by a single side (each hex has ≥1 dot and the same owner).

## How to run
Pick any one of the following methods.

- **Open in browser**: just open the HTML file (for example `src/index.html`) directly in your browser.
- **Serve with Python** (no install needed on most systems):
  ```bash
  cd /path/to/hex-game
  python3 -m http.server 4000
  # open http://localhost:4000/src/index.html
  ```
- **Serve with Node (npx)**:
  ```bash
  cd /path/to/hex-game/src
  npx serve .
  # open the printed URL
  ```
- **Yarn/npm run command start (optional)**: if you prefer yarn or npm scripts:
  Then run:
  ```bash
  yarn start
  ```
  or
  ```bash
  npm run start
  ```

## Controls
- Click a hex to add a dot (and possibly trigger a split).
- Buff buttons in the top bar apply buffs (one per click). Using a buff does not end the player’s turn.

## Implementation notes
- **Rendering**: HTML5 Canvas.
- **Grid**: flat‑topped hexes in axial coordinates `(q, r)`; positions computed analytically for crisp rendering.
- **Ownership and color**: dots are drawn in the owner’s color; the background remains neutral. Ownership changes only through splits (or resets by bombs), not simple increments.
- **Animations**:
  - Selection flash (blue for player, red for enemy).
  - Capture flash for newly captured hexes.
  - Bomb flashes with a short red overlay fade.
- **Turn logic**:
  - If an action causes at least one split, the acting side keeps the turn; otherwise the turn passes.
  - The enemy may probabilistically chain multiple buffs before its normal move.

## Ideas / roadmap
- Sound effects and toggleable music.
- Undo/redo for last action(s).
- Difficulty levels and smarter enemy AI.
- Mobile gestures and accessibility tweaks (larger hit‑areas, color‑blind friendly palette).
- Save/restore session (localStorage).
- Level editor: variable side length, custom starting positions, puzzle mode.

## License
MIT. See `LICENSE` (or treat this as MIT if the file is missing).
