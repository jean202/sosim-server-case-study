# Sosim Server Case Study

## Overview

This repository is a portfolio-oriented case study for my work on the Sosim server backend.

The backend work had to be split across:
- an active organization repository
- a deprecated legacy repository
- an independent portfolio repository for architecture notes and evidence

That separation was not just administrative. It affected contribution flow, branch strategy, PR targets, and how JWT/Redis work could be safely ported.

## What This Repository Covers

This case study focuses on three related themes:
- repository lineage recovery after an upstream split
- JWT/Redis refresh-token hardening
- turning that engineering work into reviewable contributions and portfolio evidence

## Core Story

The original backend work no longer lived in one clean repository line.

As the codebase diverged, I had to solve two problems in parallel:
- keep contributing to the active server in a clean fork relationship
- preserve and analyze the older implementation without mixing histories

That led to a three-repository strategy:
- active contribution fork for `so-sim/server`
- legacy fork for `so-sim/Deprecated_server`
- independent portfolio repository for writeups and evidence

## Main Technical Contribution

The most substantial implementation work in this case study is the JWT/Redis refresh migration ported onto the active branch.

Highlights:
- refresh token storage redesigned to a per-user Redis hash with device separation
- refresh validation changed to use both token content and device context
- refresh and logout flows updated to handle server-side invalidation correctly
- cookie handling improved to include a device identifier and null-safe extraction
- verification helpers and migration notes added for reviewability

## Why This Work Mattered

The earlier refresh flow was workable for a simple session model, but it became weak under real session-management needs:
- multiple devices per user
- selective logout
- safer server-side invalidation
- clearer refresh-token inspection and debugging

The active branch already had a different structure from the older code line, so the job was not to transplant files mechanically. The job was to port the stronger ideas into the active repository structure without regressing its conventions.

## Reading Guide

If you want the short version:

1. Start here in `README.md`
2. Read [docs/repo-lineage.md](./docs/repo-lineage.md)
3. Read [docs/jwt-redis-hardening.md](./docs/jwt-redis-hardening.md)
4. Check [docs/pr-index.md](./docs/pr-index.md)

## Document Index

- [docs/repo-lineage.md](./docs/repo-lineage.md)
  Explains how the active repo, deprecated repo, and portfolio repo were separated and why that was necessary.

- [docs/jwt-redis-hardening.md](./docs/jwt-redis-hardening.md)
  Describes the JWT/Redis migration problem, the final design, the reviewable commit split, and remaining improvement opportunities.

- [docs/pr-index.md](./docs/pr-index.md)
  Tracks contribution units, current PR-ready work, and review status.

## Concrete Evidence

Current active-branch contribution units:
- [`cf67574`](https://github.com/jean202/so-sim-server/commit/cf67574) `refactor: port multi-device JWT refresh flow to active branch`
- [`bc2af17`](https://github.com/jean202/so-sim-server/commit/bc2af17) `docs: add verification helpers for JWT refresh migration`

Validation already completed:
- `bash ./gradlew compileJava`
- `bash ./gradlew test`

## Key Lessons

### Repository history matters

Changing a remote target is not the same thing as finishing a codebase migration. Repository lineage and code architecture can diverge for a long time.

### Session design matters

JWT alone does not solve session management. Redis storage shape, cookie handling, device identity, and invalidation rules all affect the real security and operability of auth flows.

### Reviewability matters

A technically correct migration is easier to land when it is split into commits that reviewers can reason about:
- core auth/storage change
- verification and documentation follow-up

## Recommended Interview Framing

1. The project had an active backend line and a deprecated backend line, and they could no longer be treated as the same repository story.
2. I separated active contribution, legacy comparison, and portfolio documentation into distinct repositories.
3. I ported a stronger JWT/Redis refresh design into the active branch without copying the older branch blindly.
4. I left behind documents and PR-ready evidence so the work was easy to review and explain.
