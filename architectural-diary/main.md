# Architectural diary — patrick64

A narrative record of how this repository came to be the way it is,
reconstructed (2026-09-08) from the code, the assignment spec, and git
history. Companion decision records live in `decisions/`.

## The origin: a teaching base (Fall 2011)

`R-E-A-D-M-E.txt` is the actual UIUC CS 125 pair-programming lab handout
(the Eclipse project name, "PairProgramming-FA11", confirms Fall 2011). It
describes a base game "designed to help rehabilitate patients after a stroke
or brain surgery": numbers fall down the screen and the patient types them.
Students were to practice Driver/Observer pair programming by extending it
for a midterm-score bump, adding netids to the `@author` line, and committing
back to "the subversion".

The `@author` line in `RainingNumbers.java` (hsingh23, cfilla2, phann;
reviewed ddkang2, ajyoo2, yjhwang2) shows exactly that process ran, with a
review pair on top.

## The fork in the road: from typing therapy to "Patrick!"

The teams kept the class name `RainingNumbers` and the repo path but built
something entirely different on top: a mouse arcade game — dodge the sprite,
hover white checkpoint boxes, climb levels, absorb insults. The header
comment of `RainingNumbers.java` is the design notebook: TODO lists (timer,
fast/slow/±level boxes, red-kill boxes, a level-10 "play as the boss sprite"
mode, Tron-style snake trails) and an idea sign-up roster. Some shipped
(second sprite, matrix background, over-9000 mode, screen shake); most did
not. The typing-therapy game survives only as the hidden phase triggered
when the two sprites collide (`isRaining`).

For the architecture that choice produced — one file, one class, all-static
state — see `decisions/001-single-static-class-game.md`. For the graphics
layer it sits on, see `decisions/002-zen-static-facade-graphics.md`.

## 2013-12-13: SVN → git, in two minutes

Both commits in this repository are dated 2013-12-13, 48 seconds apart
(13:06:53 and 13:07:41 −0600). Commit 1 imported the whole working copy —
including the `.svn/` administrative directory, whose `pristine/` store held
svn-base duplicates of every file (which is why the "small" project weighs
in at 43 files / 4262 insertions). Commit 2 deleted all of it. `.gitignore`
already covered `.svn/`, so the metadata never came back. See
`decisions/003-svn-to-git-conversion.md`.

After that, the repository went dormant for almost thirteen years.

## 2026-09-08: documentation pass

A documentation agent analyzed both commits (worker analysis, reviewed and
approved), rewrote the two commit messages via a messages-only
`git filter-branch` (trees byte-identical; new hashes `b5ae6f8` and
`eae0234`; pre-rewrite commits preserved on local branch
`backup/pre-docs-20260908`), and added this documentation set: README.md,
AGENTS.md, CHANGELOG.md, this diary, and `prompt.md`.

## Index of decision records

- `decisions/001-single-static-class-game.md` — the whole game is one
  class with static state and a `while(true)` loop.
- `decisions/002-zen-static-facade-graphics.md` — all rendering goes
  through Zen's static facade with double buffering.
- `decisions/003-svn-to-git-conversion.md` — the two-commit git genesis
  and the `.svn/` cleanup.
