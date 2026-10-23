# Princess Dragon Slayer

A small **2D pixel-art platformer** built with **Godot 4.7 (Forward+)** — my first complete game.
Run, jump, dodge a patrolling slime, ride a moving platform, collect coins and try not to fall.

---

## About

This project is a classic single-level platformer: a knight traverses a tile-map world,
picks up coins, avoids kill zones (a pit and a slime enemy) and reaches the end of the level.
Everything — movement, animation, audio, scoring and scene reloading — is implemented in GDScript.

The game is designed around a pixel-art look: nearest-neighbour texture filtering,
a zoomed-in camera that follows the player, and a `canvas_items` / `expand` stretch mode so
the game scales cleanly to any window size.

## Features

- **Player controller** (`CharacterBody2D`) with gravity, acceleration, jumping, sprite flipping
  and `idle` / `run` / `jump` animation states.
- **Patrolling slime enemy** that walks back and forth and turns around when its
  `RayCast2D` probes hit a wall or ledge.
- **Kill zones** (`Area2D`) for the pit and for the slime: touching one prints a message,
  waits 0.6 s and reloads the current scene.
- **Collectible coins** (`Area2D` + `AnimationPlayer`) with a pickup sound effect and a
  pickup animation; every coin adds a point to the score HUD.
- **Score manager** autoload-style node (`GameManager`) that owns the score and updates the
  `ScoreLabel` UI text.
- **Moving platform** (`AnimatableBody2D` + `AnimationPlayer`) that slides between two points.
- **Camera** attached to the player with 4x zoom, smoothing, and world limits.
- **Background music** (`music.tscn`, registered as an autoload) playing on the `Music` audio bus,
  plus SFX for jumping, coins, hurt, power-ups and taps.
- **In-level tutorial labels** ("Space to jump!", "Falling hurts...", …).
- **Windows Desktop export preset** already configured.

## Controls

| Action | Keys |
| --- | --- |
| Move left | `A` or `←` |
| Move right | `D` or `→` |
| Jump | `Space` (also `ui_accept`) |

Custom input actions (`move_left`, `move_right`, `jump`) are defined in **Project → Project Settings → Input Map**.

## Project structure

```
.
├── project.godot            # Godot project configuration (main scene, autoloads, input map)
├── export_presets.cfg       # Windows Desktop export preset
├── default_bus_layout.tres  # Audio bus layout (Music / SFX buses)
├── assets/
│   ├── fonts/               # PixelOperator8 (regular + bold)
│   ├── music/               # time_for_adventure.mp3
│   ├── sounds/              # coin, jump, hurt, explosion, power_up, tap
│   └── sprites/             # knight, slime, coin, fruit, platforms, world tileset
├── scenes/
│   ├── game.tscn            # Main level: tile map, player, coins, platforms, slime, HUD
│   ├── player.tscn          # Knight (CharacterBody2D + animations + collision)
│   ├── silme.tscn           # Slime enemy (sprite, ray casts, kill zone)
│   ├── coin.tscn            # Coin (Area2D + sound + pickup animation)
│   ├── platform.tscn        # Moving platform (AnimatableBody2D)
│   ├── killzone.tscn        # Reusable kill zone (Area2D + Timer + reload logic)
│   └── music.tscn           # Autoloaded background music player
└── scripts/
    ├── player.gd            # Movement, jumping, animation control
    ├── silme.gd             # Patrol logic driven by ray casts
    ├── killzone.gd          # Death handling + scene reload timer
    ├── coin.gd              # Adds a point and plays the pickup animation
    └── game_manager.gd      # Score tracking + HUD label updates
```

## How to run

1. Install [Godot 4.7+](https://godotengine.org/download) (standard version, Forward+ renderer).
2. Open Godot → **Import** → select this folder's `project.godot`.
3. Press **F5** (Play) to run the main scene (`scenes/game.tscn`).

### Exporting

A **Windows Desktop** preset is already set up in `export_presets.cfg`.
In the editor: **Project → Export… → Windows Desktop → Export Project**.
To run without the editor, build the templates once via **Manage Export Templates**.

## Tech notes

- Engine: Godot 4.7, Forward+ rendering, D3D12 on Windows, Jolt Physics configured.
- Physics layers: layer **1** = world/tile map, layer **2** = player.
  Areas (`killzone`, `coin`) use `collision_mask = 2` so only the player triggers them.
- Signals are connected in the scene files (`body_entered` on areas, `timeout` on the kill-zone timer).

## Credits

- Music: *Time for Adventure* (background track).
- Fonts: PixelOperator8 by Jayvee Enaguas (CC BY-SA / free for game use).
- Sprites & sound effects: free pixel-art asset packs collected during learning.
- Game design, level layout and all GDScript code by me.
