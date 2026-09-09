# patrick64 — "Patrick!" (a.k.a. RainingNumbers / PatrickV8Game)

A tiny Java arcade game from UIUC **CS 125** (Fall 2011 pair-programming lab,
converted from SVN to git in 2013). You dodge a sprite with your mouse,
collect white checkpoint boxes, and climb levels while the game taunts you
with random insults when you lose. As the original readme put it: *"Behold.
The best game you will EVER play. A joy. A true joy to make."* — and yes,
there are lots of easter eggs ("LOTS of eggs in the problem").

The game began life as the lab's base example — a stroke-rehabilitation game
where you type falling numbers (see `R-E-A-D-M-E.txt`, the original assignment
spec). The pair-programming teams extended it into "Patrick!", and the
falling-numbers typing mode still survives inside as a hidden phase.

## Features

- **Mouse-dodge arcade core**: sprite(s) chase your cursor; touch = you lose
  (fade-to-red, moon backdrop, random insult from `x.txt`).
- **Checkpoint progression**: hover the white box (counted down on its face);
  collect enough per level to win, bounce the `win.jpg` image, advance.
- **Escalating difficulty**: second chaser sprite at level 4+, speed grows
  with level, scrolling background images from level 2+.
- **Hidden falling-numbers mode**: when the two sprites collide, a random
  5-digit number rains down — type it before it hits the bottom (the rehab
  game lives on).
- **Easter eggs / debug keys**: `Tab` skip to level 3, `Up` +4 levels, `F1`
  jump to level 41, `Windows` key jumps toward the secret "OVER 9000" level,
  `Delete` toggles the scrolling background, `Esc` toggles screen shake,
  `P` pause, `R` reset, `I` instructions.
- **Level-gated taunts** ("Welcome to the Real World ~ Neo", "IT'S OVER
  9000!!!", …) via `printMsg`.

## Stack

- **Java** (written for the Java 7 era; no external dependencies, no build
  tool — plain `javac`/`java`, or open the project in Eclipse/BlueJ).
- **AWT/Swing** via `Zen.java`, the CS 125 teaching graphics library
  (static-facade API, 640×480 double-buffered window created lazily on first
  draw, image caching, mouse/keyboard input; works as applet or application).

## Quickstart

Run from the repo root — the game loads `x.txt`, `*.gif`, `*.jpg` from the
working directory, so you must be in it:

```sh
javac RainingNumbers.java Zen.java
java RainingNumbers
```

(The old `Readme`'s `java -c RainingNumbers.java` was a typo for `javac`,
and omitted `Zen.java`.) A window opens, click to start.

No environment variables are required.

## Repository structure

```text
RainingNumbers.java   the game (~570 lines, single class, static state)
Zen.java              Zen graphics library (~940 lines, static facade)
x.txt                 one insult per line (lose & win screens)
win.txt               quotes file — loaded code is commented out; unused
1.jpg … 4.jpg         scrolling background strips (matrix mode)
matrix.jpg            screen-shake background ("seizure" mode)
moon.jpeg             lose-screen backdrop
win.jpg               bouncing win image
9000.gif              OVER-9000 level background
sprite1.gif/sprite2.gif  the chaser sprites
R-E-A-D-M-E.txt       original CS 125 pair-programming assignment spec
PatrickV8Game.ctxt, Zen.ctxt   BlueJ metadata
.classpath, .project  Eclipse project ("PairProgramming-FA11")
```

## History

Everything landed on 2013-12-13 when the SVN repo was converted to git: the
full project import, then removal of the accidentally committed `.svn/`
metadata. Commit messages were cleaned up (messages-only rewrite) and this
documentation set added on 2026-09-08 — see `CHANGELOG.md` and
`architectural-diary/` for the full story.
