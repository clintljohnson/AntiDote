# Antidote

A Dr. Mario–style falling-capsule puzzle game in a single HTML file. You drop two-color capsules into a glass isolation bottle and line up colors to wipe out the viruses growing inside it.

Everything is drawn live in the browser: glossy lit capsules, animated viruses that follow the falling pill with their eyes, particles, screen shake and sound effects made in code. There are no images, audio files, build steps or dependencies.

## Getting started

1. Download `antidote.html`.
2. Open it in a modern browser (Chrome, Edge, Firefox or Safari).
3. Pick a starting level and press **Start game** or **Enter**.

The page loads three Google Fonts (Unbounded, Figtree, JetBrains Mono). Without an internet connection it still plays, using system fonts.

## How to play

- Capsules fall into the bottle from the opening at the top. Each capsule has two halves, and each half is red, blue or yellow.
- Line up **four or more of the same color**, across or down, to dissolve them. Viruses count toward a line just like capsule halves.
- When half of a capsule is cleared, the other half becomes a single piece and falls. Pieces left hanging by a clear also fall, which can set off **chain reactions**.
- Clear every virus to sterilize the bottle and move on to the next level.
- The game ends when there's no room for a new capsule at the top.
- The opening at the top pulses red when your stack is getting close to it.

### Scoring

| Cleared piece  | Points |
| -------------- | ------ |
| Virus          | 200    |
| Capsule half   | 100    |

Each step of a chain multiplies the points for that step by the chain number: the first clear scores ×1, the clear it triggers ×2, then ×3 and so on.

### Levels

You can start on any level from 0 to 20. Higher levels add more viruses, spread them higher up the bottle and drop capsules faster.

| Level | Viruses | Drop interval | Speed |
| ----- | ------- | ------------- | ----- |
| 0     | 4       | 1.00 s        | Low   |
| 1     | 8       | 0.85 s        | Low   |
| 2     | 12      | 0.70 s        | Low   |
| 3     | 16      | 0.58 s        | Med   |
| 4     | 20      | 0.45 s        | Med   |
| 5     | 24      | 0.35 s        | Med   |
| 6     | 28      | 0.25 s        | High  |
| 7     | 32      | 0.18 s        | High  |
| 8     | 36      | 0.13 s        | High  |
| 9+    | 4 + 4 × level (max 84) | 0.10 s | High |

## Controls

### Keyboard (default)

| Action        | Keys       |
| ------------- | ---------- |
| Move left     | ←          |
| Move right    | →          |
| Rotate left   | Z          |
| Rotate right  | X or ↑     |
| Soft drop     | ↓          |
| Hard drop     | Space      |
| Pause         | P          |
| Quit run      | Q          |
| Sound on/off  | M          |

**Esc** pauses and resumes, and **Space** resumes while the game is paused. These two always work and can't be remapped.

### Remapping keys

Click **Edit** in the Controls panel. The game pauses and every action shows two key slots.

- Click a slot, then press the key you want.
- **Esc** cancels and **Backspace** clears the slot.
- If the key was already used by another action, it moves to the new one.
- **Reset** restores the defaults. Click **Done** when you're finished.

Your keys are saved in the browser, and the on-screen buttons and hints update to match them.

### Mouse (on the bottle)

| Action        | Input                     |
| ------------- | ------------------------- |
| Rotate right  | Left click                |
| Rotate left   | Middle click              |
| Hard drop     | Right click               |
| Slide         | Scroll wheel: down moves right, up moves left |

Trackpad scrolling works too.

### Touch

- Drag across the bottle to move the capsule.
- Tap to rotate.
- Flick down to hard drop.

The on-screen pad in the Controls panel also works with a mouse or a finger. Holding the arrow buttons repeats the move.

## Features

- **Fills the screen:** the bottle uses the full height of the window and rescales live when you resize it. On narrow screens and phones the layout stacks into one column.
- **High scores:** a top-10 list with the date and level of each run. A run is posted when the game ends or when you quit with a score above zero. The game-over screen tells you when you've set a new high score or made the board.
- **Sound:** effects for moving, rotating, landing, clears, chains, level clears and game over. Use the speaker button or **M** to mute. Browsers only allow sound after your first click or key press.
- **Pause screen:** a blurred overlay with a Resume button.
- **Side panels:** virus counts for each color, a bar showing how much of the infection is left, the current speed, your top chain and a preview of the next capsule.
- **Reduced motion:** if your system asks for reduced motion, screen shake, wobble and confetti are toned down.

## Saved data

Everything is stored in your browser's `localStorage`, so it stays on the device and browser you play in. Clearing your site data resets it.

| Key               | Contents                                      |
| ----------------- | --------------------------------------------- |
| `antidote-scores` | Top-10 high scores                            |
| `antidote-keys`   | Your key bindings                             |
| `antidote-muted`  | Whether sound is muted                        |
| `viral-load-best` | Best score from an earlier version. It's carried into the high scores the first time the list is empty. |

## Under the hood

- **One file:** the HTML, CSS and JavaScript all live in `antidote.html`.
- **Graphics:** everything is drawn with the Canvas 2D API at your screen's full pixel density. Capsule halves and virus bodies are pre-rendered as sprites with their own lighting and shadows, and they're rebuilt whenever the board size changes.
- **Animation:** one `requestAnimationFrame` loop handles drawing, effects and the drop timer. Falling and settling pieces are smoothed between grid cells.
- **Sound:** every effect is generated with the Web Audio API from oscillators and filtered noise, running through a compressor.

### Tweaking the game

These constants near the top of the script are the easiest places to start:

| What                  | Where in the script                          |
| --------------------- | -------------------------------------------- |
| Bottle size           | `COLS` and `ROWS`                            |
| Colors                | `PAL`                                        |
| Drop speed by level   | `SPEEDS`                                     |
| Virus count and height| `placeViruses()`                             |
| Scoring               | `countScore()`                               |
| Default keys          | `DEFAULT_KEYS`                               |
| Sound effects         | the `SOUNDS` object inside `Sfx`             |
