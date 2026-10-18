# Asteroids

A classic Asteroids-style arcade game built with Python and [pygame](https://www.pygame.org/). Pilot a triangular ship, blast asteroids into smaller pieces, and survive as long as you can.

Built as part of the [Boot.dev](https://www.boot.dev) Python curriculum.

## Features

- **Ship controls** with smooth rotation and forward/backward thrust
- **Shooting** with a 0.3-second cooldown (rate limiting)
- **Asteroid splitting**: large asteroids break into two smaller, faster ones at random angles; the smallest ones are destroyed
- **Endless spawning** from random screen edges at random speeds and sizes
- **Circle-based collision detection** between the ship, shots, and asteroids
- **Game over** when an asteroid hits the ship
- **Structured logging** of game state and events to JSONL files for debugging and automated testing

## Controls

| Key     | Action                |
| ------- | --------------------- |
| `A`     | Rotate left           |
| `D`     | Rotate right          |
| `W`     | Move forward          |
| `S`     | Move backward         |
| `Space` | Shoot                 |

## Getting Started

### Requirements

- Python (version in `.python-version`)
- [uv](https://docs.astral.sh/uv/) for dependency management

### Run the game

```bash
git clone https://github.com/ABman-FireBird/Asteroids.git
cd Asteroids
uv run main.py
```

`uv` installs pygame automatically on the first run.

## Project Structure

| File               | Purpose                                                            |
| ------------------ | ------------------------------------------------------------------ |
| `main.py`          | Game loop, sprite groups, collision handling                       |
| `circleshape.py`   | Base class for all game objects, including `collides_with()`       |
| `player.py`        | The ship: drawing, movement, rotation, shooting and cooldown       |
| `asteroid.py`      | Asteroid behavior, including `split()`                             |
| `asteroidfield.py` | Spawns asteroids at random screen edges                            |
| `shot.py`          | Bullets fired by the player                                        |
| `constants.py`     | Tunable settings (screen size, speeds, radii, cooldown)            |
| `logger.py`        | Writes `game_state.jsonl` and `game_events.jsonl`                  |

## How It Works

- Every game object inherits from `CircleShape`, a `pygame.sprite.Sprite` with a position, velocity, and radius.
- Objects are organized into pygame **groups** (`updatable`, `drawable`, `asteroids`, `shots`). Each frame, the loop updates everything in `updatable` and draws everything in `drawable`.
- Two circles collide when the distance between their centers is less than or equal to the sum of their radii.
- When a shot hits an asteroid, `split()` removes it and, if it is larger than the minimum radius, spawns two smaller asteroids with velocities rotated by a random 20-50 degrees in opposite directions and scaled up by 1.2x.

## Configuration

Gameplay values live in `constants.py`, for example:

```python
PLAYER_SPEED = 200
PLAYER_TURN_SPEED = 300
PLAYER_SHOOT_SPEED = 500
PLAYER_SHOOT_COOLDOWN_SECONDS = 0.3
ASTEROID_SPAWN_RATE_SECONDS = 0.8
```

Change them to make the game easier or harder.

## Possible Improvements

- Score and lives system
- Screen wrapping for the ship
- Sound effects and a start/game-over screen
