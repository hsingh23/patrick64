# One-shot recreation prompt — patrick64

Feed this prompt to a coding agent to recreate this repository from scratch
(one game file, one graphics library, assets, assignment context). It is the
condensed essence of the 2011–2013 project as documented in 2026.

---

Create a small Java game project called "Patrick!" (internally also known as
RainingNumbers / PatrickV8Game) — a UIUC CS 125 pair-programming lab game
that grew out of a stroke-rehabilitation typing example. No build system, no
dependencies, no tests; two Java files plus text/image assets in the repo
root, runnable with plain `javac`/`java` from the project directory.

## File 1 — Zen.java (graphics library, ~940 lines)

Write a teaching graphics library as a single class `Zen extends JApplet`
with a **static facade API**. Requirements:

- Runnable as application: on the first draw call from the user's thread,
  lazily create a `JFrame` (default 640×480, EXIT_ON_CLOSE) holding an
  offscreen `BufferedImage` back buffer; keep per-thread instances in a
  `ThreadLocal` map. Also retain a working applet `init()` path.
- Static drawing API over the back buffer's `Graphics2D`: `drawImage`
  (by filename or `Image`, with optional width/height), `drawLine`,
  `drawText`, `drawArc`, `fillOval`, `fillRect`, `setColor(int,int,int)`,
  `setColor(Color)`, `setFont(String)` (font spec like "Times-22"),
  `bound(value,min,max)`.
- Double buffering: `flipBuffer()` swaps front/back and repaints.
- Input: `getMouseX/Y`, `getMouseClickX/Y/Time`, `getMouseButtonsAndModifierKeys`,
  `waitForClick()`, `isKeyPressed(char)`, `isVirtualKeyPressed(int keyCode)`,
  `getEditText()/setEditText(String)` (a hidden text field for typed input).
- Timing/misc: `sleep(int ms)`, `isRunning()`, `closeWindow()`,
  `getWindowScreenShot()`, `set/getClipboardImage`, `getCachedImage(filename)`
  with a HashMap image cache, `toBufferedImage`.
- Implementation: AWT/Swing only; `PixelGrabber`-era utilities acceptable;
  verbose Javadoc tone of a course handout.

## File 2 — RainingNumbers.java (the game, ~570 lines)

One class, all state `private static`, everything driven from
`main(String[] args)`. Header block comment: `@author`/`@reviewed` netid
lines (pair-programming roster), a description ("get boxes with your mouse
and avoid the player from touching you… insults you when you lose"), plus
TODO and idea-signup comment lists — treat the header as the design
notebook.

Gameplay loop (`while (true)`, `Zen.sleep(25 - 2*speed)` per frame):

1. `moveSprite1()`: sprite at (x,y) steps toward the mouse cursor each
   frame; if mouse comes within ±5 px of the sprite (or the second sprite),
   `youLose()`.
2. A white 50×50 checkpoint box sits at (cx,cy) showing the remaining
   count; hovering the mouse inside it increments `ctr` and calls
   `nextCheckpoint()` (random new position). When `ctr == checkpoint`,
   `youWin()`.
3. `youLose()`: fade the screen white→black, red taunt text ("Aww Gee - Was
   it Really That Hard? I mean REALLY?"), draw `moon.jpeg` fullscreen, print
   a random line of `x.txt` (word-wrapped at ~48 chars via `cutString`),
   wait 5s, wait for click, reset to level 0.
4. `youWin()`: cyan fade, "You WIN.", then a physics animation of `win.jpg`
   (300×300, gravity with 0.9-restitution bounce, random horizontal
   velocity, wall bounce), another random `x.txt` line, level++, checkpoint
   += level, speed++. Then level-gated taunt screens (`printMsg`):
   level 3 "Press Tab to skip lame part", 4 aspirin joke, 6 breath-holding,
   10 screenshot, 12 "I hope you don't have to pee", 42 "Welcome to the
   Real World ~ Neo", 9001 "IT'S OVER 9000!!!".
5. Level features: `matrixIsON` from level 2 → tile `1.jpg`–`4.jpg` into a
   vertical scroll whose speed is `Math.pow(1.3, level) + 10`; second
   chaser sprite `sprite2.gif` from level 4 (`moveSprite2`, steps toward
   mouse with its own direction pair); `o9` at level ≥ 9000 → draw
   `9000.gif` fullscreen with jitter.
6. Hidden rain mode: when the two sprites come within ±15 px,
   `rainNumber()` picks a random ≤5-digit number at a random x, `drawNumber()`
   red-on-black falls 1 px/frame; `checkRain()` consumes typed chars from
   `Zen.getEditText()` matching `rainString` front char; empty string wins
   the rain; number passing the bottom = lose. First activation shows
   "Write Dat Numbahahahahaha" and waits for click.
7. `checkKeys()`: P pause (waitForClick), R reset (initialize), I
   instructions, Tab → level 3/speed 3 + youWin, Up → +4 levels + youWin,
   F1 → level 41/speed 10, Windows key → level 8999/speed 25 + youWin,
   Delete → toggle matrix background, Escape → toggle `shiftmix` (draw
   `matrix.jpg` fullscreen at random jitter offsets). Include the original
   commented-out block for Up/Down game-type switching.
8. `drawCheckpoint()` also draws a random red oval "distraction" when
   elapsed time passes `actionTime`. Keep dead code: `genNewTime`/
   `checkTime` stubs, commented-out `win.txt` loader and neo cheat.
9. Instructions screen: "PATRICK! V16 By: Harsh Singh & Philip Hann", goal
   text, "Click to start". HUD `showLevel()`: "L: level Chkpts: … ctr: …
   Speed: …" in red on a black strip.

## Assets (create placeholders or small originals)

`sprite1.gif`, `sprite2.gif` (chaser sprites); `1.jpg`–`4.jpg` (background
strips), `matrix.jpg` (busy fullscreen backdrop), `moon.jpeg` (lose
backdrop), `win.jpg` (bounce image), `9000.gif` (jittered backdrop);
`x.txt` (one playful insult per line, ~84 lines — keep them school-safe
dad-joke quality); `win.txt` (quotes file; loader commented out);
`R-E-A-D-M-E.txt` (assignment spec: pair-programming technique, Driver/
Observer roles, the rehab-game brief, known bugs incl. the
StringIndexOutOfBoundsException on extra keystrokes in rain mode);
`Readme.md` (two joke lines + run commands); Eclipse `.project`
(name "PairProgramming-FA11") and `.classpath`; BlueJ `.ctxt` files;
`.gitignore` (`*.class`, `.svn/`, `*.jar|war|ear`, `.mtj.tmp/`).

## Verification

`javac RainingNumbers.java Zen.java && java RainingNumbers` from the
project root: window opens, instructions render, click starts, boxes
collect, lose/win screens show `x.txt` lines, Tab/Up/Delete/Esc keys work,
collision of sprites triggers rain mode.
