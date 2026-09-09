# ADR 002 — All rendering goes through Zen's static facade

- **Date:** Fall 2011 (Zen library authored by the course staff, angrave)
- **Status:** accepted, shipped (library vendored, never modified)

## Decision

Vendor `Zen.java` (~940 lines) alongside the game and use only its static
API for every graphical and input concern: `Zen.drawImage/drawText/fillRect/
fillOval/setColor/setFont`, `Zen.getMouseX/Y`, `Zen.waitForClick()`,
`Zen.isVirtualKeyPressed(...)`, `Zen.getCachedImage(...)`, and
`Zen.flipBuffer()` per frame. The game never touches AWT directly except
for `Image` handles and one `KeyEvent` constant import.

## Rationale

Zen is the CS 125 teaching library: a `JApplet` that can also run as an
application, lazily creating a 640×480 `JFrame` with an offscreen buffer on
the first draw call from the main thread (thread-local `ZenInstance` map).
Students get double buffering, input polling, and image caching without
learning Swing. Vendoring it (rather than referencing a course jar) made
the pair's checkout self-contained in SVN.

## Consequences

- **Good:** zero flicker for free (back buffer + `flipBuffer()`), which the
  assignment lists as bug #1 for naive implementations — this game avoided
  it by construction.
- **Good:** input, timing (`Zen.sleep`), and images all flow through one
  well-tested seam; assets load by filename and are cached.
- **Bad:** the game and library are welded together at the package root —
  `RainingNumbers.java` cannot compile without `Zen.java` in the same
  directory, and assets must be in the working directory at runtime.
- **Quirk:** applet heritage shows — `Zen` has an `init()` lifecycle and
  can be embedded; the game only uses the application path.

## Alternatives considered

Direct Swing/AWT coding was the "hard mode" the library exists to spare
students from; no alternative was ever considered here.
