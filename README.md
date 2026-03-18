# Sokoban

A graphical Sokoban puzzle game built with Python and pygame.

Push all boxes onto the goal squares to solve each puzzle.

## Requirements

- Python 3.10+
- pygame (`pip install pygame-ce`)

## Running the Game

```bash
python3 sokoban.py
```

A pygame window will open displaying the level. Use arrow keys or WASD to move, and close the window or press Q to quit.

## Controls

| Key              | Action                  |
|------------------|-------------------------|
| Arrow keys / WASD | Move the player        |
| U                | Undo last move          |
| R                | Reset the level         |
| Q                | Quit                    |

## How the Board Works

The board is a grid of colored tiles:

| Color        | Meaning                              |
|--------------|--------------------------------------|
| Dark gray    | Wall — blocks movement               |
| Light gray   | Floor — open space                   |
| Yellow dot   | Goal — a box must be placed here     |
| Brown        | Box — push it onto a goal            |
| Green        | Box on goal — correctly placed       |
| Blue         | Player — that's you                  |

## Project Structure

Everything lives in a single file: `sokoban.py`.

### What's already built

- **Level parsing** (`_parse_level`) — converts a list of strings into a 2D grid and locates the player. The player's position is tracked separately from the grid; the grid stores what is underneath the player (floor or goal).
- **Rendering** (`render`) — draws the board as colored rectangles using pygame, with a HUD showing move count and controls.
- **Grid access** (`_get_cell`) — safe accessor that returns a wall character for out-of-bounds coordinates.
- **Player movement** (`move`) — handles directional input, checks for walls, delegates to `push_box` when encountering a box, and updates the player position.
- **Box pushing** (`push_box`) — checks whether a box can be pushed (destination must be floor or goal), then updates the grid to move the box.
- **Game loop** (`run`) — pygame event loop that maps keyboard input to game actions and renders each frame at 30 FPS. Displays a win overlay when the puzzle is solved.

### What you need to implement

Search for `TODO(peer)` in `sokoban.py` to find all four stubs. Each has a detailed docstring and hints.

#### 1. `check_win()` — Win condition detection

Determine whether all goals are covered by boxes. In grid terms, if no cell contains `GOAL` (`.`), the puzzle is solved — because a goal with a box on it is stored as `BOX_ON_GOAL` (`*`).

#### 2. `undo()` — Undo last move

Restore the previous game state from `self.history`. Each history entry is a tuple of `(grid_copy, player_row, player_col, move_count)`. You will also need to add a snapshot-saving line inside the `move()` method at the marked TODO comment, so that state is captured before each move. Use `copy.deepcopy(self.grid)` to avoid aliasing.

#### 3. `increment_move_count()` — Move counter

Increment `self.move_count` by 1. This is called automatically by `move()` after each successful move. The count is already displayed by `render()`.

#### 4. `reset_level()` — Level reset

Restore the level to its initial state by re-parsing `self.initial_level` using the existing `_parse_level()` method. Reset `self.move_count` to 0 and `self.history` to an empty list.

### Key design details for implementers

- **Player is not in the grid.** The grid cell at the player's position holds what's underneath them (`FLOOR` or `GOAL`). The player's coordinates are in `self.player_row` and `self.player_col`.
- **Box-on-goal is a distinct cell type.** When a box sits on a goal, the cell is `BOX_ON_GOAL` (`*`), not `BOX`. When checking for uncovered goals, look for cells equal to `GOAL` (`.`).
- **`self.history` and `self.move_count` are already initialized** in `__init__`. You only need to write the logic that uses them.
- **No rendering code needed.** All four stubs are pure game logic — update `self.grid` and the instance attributes, and `render()` handles the display automatically.
