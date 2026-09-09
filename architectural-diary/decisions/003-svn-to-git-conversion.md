# ADR 003 — Genesis as a two-commit SVN→git conversion

- **Date:** 2013-12-13 (both commits, 48 seconds apart)
- **Status:** accepted, shipped; messages improved 2026-09-08
  (messages-only rewrite, trees unchanged)

## Decision

Preserve the project's history by importing the SVN working copy wholesale
into a fresh git repository, then immediately delete the accidentally
included `.svn/` metadata in a second commit — rather than using
`git svn clone` (which would have mapped SVN revisions to git commits) or
starting history from a clean snapshot.

## Rationale (reconstructed)

The pre-2013 SVN history was course-server scratch space, not worth
faithful migration; what mattered was the final tree. `git init` + `git add .`
inside the existing working copy was the path of least resistance — and it
swept in `.svn/` (22 files, 2124 deletions' worth of pristine svn-base
copies and the `wc.db` SQLite database) because the brand-new `.gitignore`
came in the same commit. The very next commit, 48 seconds later, removed
it all.

## Consequences

- **Good:** the complete final tree exists at the root commit
  (`b5ae6f8`), so `git log --reverse` tells the whole story in two commits.
- **Bad:** no development history — the interesting Fall-2011 evolution
  (base rehab game → Patrick!) is invisible; only the source header
  comments record it.
- **Oddity:** commit 1's original message, "git ignore", described 9 lines
  of a 4,262-line change; commit 2's "converts svn repo to git" described
  the cleanup rather than the conversion. Both were rewritten on
  2026-09-08 to conventional-commit form:
  - `b5ae6f8` — `chore: import RainingNumbers game project with Zen
    library and assets`
  - `eae0234` — `chore: remove accidentally committed .svn working-copy
    metadata`
  Pre-rewrite objects remain on local branch `backup/pre-docs-20260908`
  (`e9c019f`, `c9181a4`).

## Alternatives considered

`git svnclone`/`svn2git` (rejected: course SVN history was throwaway and
the server was going away); single squashed commit (rejected implicitly —
the author chose to keep the metadata cleanup as a separate change, which
usefully documents the mistake).
