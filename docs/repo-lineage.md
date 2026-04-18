# Repository Lineage

## Current Repository Map

As of April 18, 2026, the Sosim backend work is organized across three repositories and two local service worktrees.

| Role | Local path | GitHub repository | Relationship |
| --- | --- | --- | --- |
| Active contribution repo | `/Users/jean325/IdeaProjects/study/sosim/server_ver2` | `jean202/so-sim-server` | Fork of `so-sim/server` |
| Legacy comparison/backport repo | `/Users/jean325/IdeaProjects/study/sosim/deprecated-server` | `jean202/sosim-deprecated-server` | Fork of `so-sim/Deprecated_server` |
| Portfolio repo | `/Users/jean325/IdeaProjects/study/sosim/portfolio/sosim-server-case-study` | `jean202/sosim-server-case-study` | Independent repository |

## Why This Split Exists

The codebase history no longer fits into one clean repository line.

Two different realities had to be managed at once:
- the currently active organization server at `so-sim/server`
- the older implementation line preserved at `so-sim/Deprecated_server`

That created a practical portfolio problem:
- active contribution work needed a clean fork relationship to the current organization repository
- legacy analysis and backport work needed a separate fork relationship to the deprecated repository
- architecture writeups and project narrative needed an independent, presentation-oriented repository

## Local Branch Strategy

### `server_ver2`

This worktree intentionally keeps both the active line and a preserved historical line.

- `active-develop`
  - tracks `origin/develop`
  - used for active contribution to `so-sim/server`
- `develop`
  - preserved pre-split local history
  - kept as reference because earlier JWT/Redis work was developed there
- `legacy-develop`
  - explicit alias for the preserved historical line
- `upstream-develop`
  - read-only comparison branch for `so-sim/server`
- `upstream-main`
  - read-only comparison branch for `so-sim/server` main line

### `deprecated-server`

- `develop`
  - tracks `upstream/develop`
  - used for legacy comparison and future backports
- `upstream-develop`
  - read-only comparison branch
- `upstream-main`
  - read-only comparison branch

## Important Timeline

### 1. Legacy implementation line

The older server implementation is still represented by `so-sim/Deprecated_server`.

This line contains the earlier JWT/Redis model and older API structure, and it is useful for:
- comparison
- regression analysis
- historical documentation
- selective backports

### 2. Active implementation line

The active organization repository is `so-sim/server`.

This line has a more reorganized module layout and different response/JWT conventions. It cannot be treated as a trivial continuation of the older repository layout.

### 3. Portfolio separation

Instead of mixing documentation into either service repository, the portfolio narrative now lives in its own repository:
- `jean202/sosim-server-case-study`

This separation makes it easier to explain:
- what changed
- why the migration was necessary
- which repository each contribution belongs to

## Key Lessons From The Lineage Split

### Remote URLs are not architecture

A repository can point to a new remote long before its code structure truly matches the new upstream.

### Active and legacy work need different forks

Trying to use one personal repository for both active and deprecated upstreams creates ambiguous history, confusing PR targets, and weak portfolio presentation.

### Portfolio material benefits from independence

A case-study repository should optimize for explanation and evidence, not for production branch structure.

## Reading Order

Recommended reading order for this repository:

1. [README.md](../README.md)
2. [jwt-redis-hardening.md](./jwt-redis-hardening.md)
3. [pr-index.md](./pr-index.md)
