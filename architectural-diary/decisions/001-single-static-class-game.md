# ADR 001 — The game is a single class with all-static state

- **Date:** Fall 2011 (inferred from project name and assignment spec)
- **Status:** accepted, shipped
- **Context:** CS 125 students, learning Java, extending a lab base game
  under pair programming with weekly demo deadlines.

## Decision

Write the entire game as one class (`RainingNumbers`, ~570 lines) with no
instances: all game state is `private static` fields (`x, y, x2, y2, level,
speed, ctr, checkpoint, startTime, …`), all behavior is static methods, and
`main()` owns a single `while (true)` game loop that steps sprites, checks
collisions, draws, and sleeps `25 − 2×speed` ms per frame.

## Rationale

This is the natural shape of code written by students one lab past "static
methods": no object graph to design, state trivially reachable from every
helper, and the file still compiles after any pair swaps seats mid-typing.
The lab format (driver/observer swapping every 5–10 minutes) rewards flat,
obvious code over clever structure.

## Consequences

- **Good:** extremely readable as a script; every feature (win screens,
  rain mode, debug keys) is one static method away from the loop.
- **Good:** the method names form a de facto module map — `youLose`,
  `youWin`, `nextCheckpoint`, `checkRain`, `moveSprite1/2`, `checkKeys`.
- **Bad:** nothing is unit-testable in isolation (static mutable state,
  `Zen` calls everywhere); the only verification is running the game.
- **Bad:** scaling levers are scattered (speed raised in `youWin`, capped
  implicitly by `Zen.sleep(25 - 2*speed)` going to zero around speed 12).
- **Note:** dead code and commented experiments are left in place on
  purpose — the header comment block doubles as the roadmap. Later agents
  should preserve that convention.

## Alternatives considered

An OO refactor (Sprite/Checkpoint/Level objects) is listed in the header
TODOs ("ObjectOrient adding sprites?") and was never done — the game
reached "done enough" for the demo and froze.
