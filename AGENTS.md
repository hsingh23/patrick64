# AGENTS.md — agent guide to patrick64

Guidance for coding agents working in this repository. Read this before
touching anything.

## What this is

A ~1,500-line, two-file Java project: `RainingNumbers.java` (the "Patrick!"
mouse arcade game from a UIUC CS 125 pair-programming lab) and `Zen.java`
(the course's teaching graphics library). No build system, no tests, no
dependencies, no CI. Plus ~12 image/text assets the game loads from the
working directory at runtime.

## Commands

```sh
# Build (produces .class files in repo root — .gitignore'd; never commit them)
javac RainingNumbers.java Zen.java

# Run (MUST be from repo root: game opens x.txt and images by relative path)
java RainingNumbers

# History
git log --oneline --decorate
```

There is no test suite, no linter, no package manager. Verification is:
it compiles, it launches, the window opens, click-to-start works, a level
plays. `Zen` opens a 640×480 AWT window lazily on the first draw call —
running requires a display (headless CI will not work).

## Architecture map

```text
RainingNumbers.main()          everything lives in one class, all state static
 ├─ getInsultsArray()          loads x.txt into ArrayList<String> insults
 ├─ loadInstructions()         title screen; Zen.waitForClick() to start
 ├─ initialize()               per-level reset (sprites, checkpoint, timers)
 └─ while(true) game loop      ~60 iterations/sec via Zen.sleep(25 - 2*speed)
     ├─ moveSprite1/2()        sprites step toward the mouse cursor
     ├─ collision checks       mouse-touch → youLose(); sprites collide → rain mode
     ├─ backgrounds            matrix scroll (level≥2), shake (Esc), 9000.gif (≥9000)
     ├─ checkRain()/drawNumber()  hidden typing mini-game (rehab-game remnant)
     ├─ checkpoint hover test  mx,my inside box → ctr++, nextCheckpoint()
     ├─ checkKeys()            P/R/I/Tab/Up/F1/Windows/Delete/Esc handlers
     ├─ drawCheckpoint()/drawImage(sprite1.gif)
     └─ Zen.flipBuffer()       double-buffer swap each frame

Zen.java                       static facade over AWT/Swing (author: angrave)
 ├─ create()/lazy getInstance()  thread-local ZenInstance, JFrame w/ buffer
 ├─ draw*/fill*/setColor/setFont  back-buffer Graphics2D wrappers
 ├─ flipBuffer()                 swap + repaint
 └─ input: getMouseX/Y, waitForClick, isVirtualKeyPressed, get/setEditText
```

## Conventions (as the codebase actually does it)

- Single class, `public static` everything, static mutable fields for game
  state (`x, y, x2, y2, level, speed, ctr, checkpoint, …`). Match this style
  for small edits; do not introduce a framework or split files gratuitously.
- Comments are extensive informal block comments at the top of
  `RainingNumbers.java` (TODOs, idea sign-ups). Keep them; they are the
  project's design notebook.
- No external libraries ever; stdlib AWT only.
- Files are loaded by relative path — assets must stay in the repo root.

## Gotchas

- **`win.txt` is dead data**: the code that loads it is commented out inside
  `getInsultsArray()`; win screens also read `x.txt`. Don't "fix" this
  without noting behavior change.
- **Known crash (inherited from the lab)**: `checkRain()` calls
  `rainString.charAt(0)` per typed char; once `rainString` is empty a further
  keystroke throws `StringIndexOutOfBoundsException`. The assignment file
  `R-E-A-D-M-E.txt` documents this as a known student-facing bug.
- **Version-number drift**: repo/dir say "V8", the in-game title says
  "PATRICK! V16", file `PatrickV8Game.ctxt` says V8. None is authoritative.
- `Zen.sleep(25 - 2*speed)` means high `speed` values crash-level toward 0ms
  sleep; the Windows-key path sets speed 25 (would go negative — guarded
  only by level flow reaching 9000 first). Treat speed as bounded [1,11].
- The Eclipse project name is "PairProgramming-FA11" though commits are from
  2013 — the repo predates its git conversion; don't "correct" dates/names.
- History was rewritten on 2026-09-08 (messages only). `backup/pre-docs-20260908`
  holds the pre-rewrite commits if old hashes are ever needed.

## Verify your changes

1. `javac RainingNumbers.java Zen.java` — clean compile, no new warnings you
   can't explain.
2. `java RainingNumbers` from repo root — window opens, instructions render,
   click starts a level, box collection increments the HUD, `P`/`R`/`I` keys
   respond.
3. `git status` — only intended files modified; no `.class` files staged
   (they are gitignored, but verify).

## Pointers

- `CHANGELOG.md` — every commit, newest first.
- `architectural-diary/` — how and why the codebase is shaped this way.
- `prompt.md` — one-shot recreation prompt for the whole project.
- `R-E-A-D-M-E.txt` — the original assignment spec (pair-programming rules,
  the base rehab game, known bugs).
