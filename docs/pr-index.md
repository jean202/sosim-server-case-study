# PR Index

## Purpose

This file tracks contribution units that are ready for review, already pushed, or planned for later submission.

## Active Server Contributions

### 1. JWT/Redis refresh migration for active branch

- Status: pushed to fork, ready to open as PR
- Upstream target: `so-sim/server`
- Base branch: `so-sim/server:develop`
- Head branch: `jean202/so-sim-server:develop`
- Compare URL: [Open compare view](https://github.com/so-sim/server/compare/develop...jean202:so-sim-server:develop)

Commits:
- [`cf67574`](https://github.com/jean202/so-sim-server/commit/cf67574) `refactor: port multi-device JWT refresh flow to active branch`
- [`bc2af17`](https://github.com/jean202/so-sim-server/commit/bc2af17) `docs: add verification helpers for JWT refresh migration`

Scope:
- multi-device refresh token storage in Redis hash form
- refresh token subject-based user identification
- refresh + device cookie issuance
- Redis invalidation on logout
- dev-profile verification endpoints
- migration notes for review

Validation:
- `bash ./gradlew compileJava`
- `bash ./gradlew test`

Related document:
- [jwt-redis-hardening.md](./jwt-redis-hardening.md)

## Legacy Server Contributions

### 1. Deprecated server backport plan

- Status: not started
- Upstream target: `so-sim/Deprecated_server`
- Planned direction: selectively backport only improvements that fit the deprecated architecture

Candidates:
- logout invalidation improvements
- cookie handling safety fixes
- explicit refresh error handling

## Portfolio Repository Changes

### 1. Case-study bootstrap

- Status: completed
- Repository: `jean202/sosim-server-case-study`
- Purpose: document architecture split, contribution evidence, and JWT/Redis design decisions

Documents:
- [README.md](../README.md)
- [repo-lineage.md](./repo-lineage.md)
- [jwt-redis-hardening.md](./jwt-redis-hardening.md)
