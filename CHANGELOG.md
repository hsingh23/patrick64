# Changelog

All notable changes to this project. Dates reflect commit dates; new entries go at the top.

> **History rewrite note (2026-09-08):** On 2026-09-08 the two original commit
> messages were improved via a messages-only `git filter-branch` rewrite
> (trees and file content unchanged, hashes changed as a result). The entries
> below use the post-rewrite commit hashes. Pre-rewrite hashes, for reference:
> `e9c019f73293e69936b3e6ff81d4d929dd84dca7` ("git ignore") and
> `c9181a4df66647cf1fbd3ffb2e63264018685091` ("converts svn repo to git").

## 2013-12-13 — `eae0234` — chore: remove accidentally committed .svn working-copy metadata

- Delete the `.svn/` directory (22 files, 2124 deletions): the pristine
  `svn-base` copies of every project file, `entries`, `format`, and the binary
  `wc.db` working-copy database, all committed one minute earlier in the
  initial import.
- No application source or assets change; the game's behavior is untouched.
- Finishes the SVN-to-git migration: the repository is now purely git-tracked,
  and `.svn/` remains covered by `.gitignore` so it stays untracked going
  forward.

## 2013-12-13 — `b5ae6f8` — chore: import RainingNumbers game project with Zen library and assets

- Initial import of the "Patrick!" CS 125 pair-programming project
  (43 files, 4262 insertions) from an Eclipse/SVN working copy.
- Game source `RainingNumbers.java`: a mouse-driven avoid-and-collect arcade
  game (dodge sprites, hover white checkpoint boxes, climb levels, get taunted
  from `x.txt` when you lose) that retains the class name of the base lab's
  falling-numbers rehabilitation game and still contains that typing mini-mode
  as a hidden phase.
- `Zen.java` graphics library (UIUC CS 125 teaching library by Paul Angrave):
  a static-facade over AWT/Swing with double buffering, mouse/keyboard input,
  and image caching, usable as applet or application.
- Eclipse project files (`.classpath`, `.project` — project name
  "PairProgramming-FA11") and BlueJ context files (`Zen.ctxt`,
  `PatrickV8Game.ctxt`).
- Image/sprite assets (`sprite1.gif`, `sprite2.gif`, scrolling background
  strips `1.jpg`–`4.jpg`, `matrix.jpg`, `moon.jpeg`, `win.jpg`, `9000.gif`)
  and text data (`x.txt` insults, `win.txt` quotes, `R-E-A-D-M-E.txt`
  assignment spec, `Readme.md`).
- Adds a `.gitignore` (`*.class`, `.svn/`, `*.jar`, `*.war`, `*.ear`,
  `.mtj.tmp/`) — although `.svn/` metadata was still accidentally committed
  in this same change and removed in the following one.
