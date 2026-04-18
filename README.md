# Sosim Server Case Study

## Overview

This repository is a portfolio-oriented case study for my work on the Sosim server codebase.

The project history split into two tracks:
- an older codebase now represented locally by `deprecated-server`
- a newer organization repository represented by `so-sim/server`

Because the repository lineage changed during later development, I organized my work around three goals:
- continue contributing to the active organization codebase
- preserve and understand the legacy implementation path
- document technical decisions in a form that is readable as a portfolio

## My Focus Area

The main technical theme of this case study is JWT and Redis session handling.

I focused on:
- refresh token storage redesign
- JWT cookie bug fixes
- active vs legacy server comparison
- contribution workflow recovery after repository history split

## Problem Background

The original server and the newer organization server do not share the same structure anymore.

This created practical issues:
- remote relationships became confusing
- active contribution targets were unclear
- old implementation details still mattered for debugging and comparison
- portfolio presentation became difficult because the code history was split across repositories

## What I Changed

### 1. Refresh token storage redesign

I changed refresh token storage from a single-token mapping to a per-user hash structure.

Before:
- key: refresh token string
- value: user id
- no meaningful device separation
- no clean multi-device support

After:
- key: `refresh:{userId}`
- field: `deviceId`
- value: refresh token
- TTL: 14 days

This gave the server a safer and more explicit base for multi-device session management.

### 2. JWT cookie bug fixes

I fixed three separate issues in the cookie-based refresh flow:
- Set-Cookie parsing logic
- refresh cookie max-age mismatch
- refresh endpoint mismatch between frontend and backend

These bugs were small individually, but together they caused unstable token refresh behavior.

### 3. Active vs legacy repository analysis

I compared:
- `server_ver2`
- `deprecated-server`
- the downloaded phase-2 server from `so-sim/server`

The result was clear:
- `server_ver2` still follows the legacy architecture more closely
- the active organization repo uses a more reorganized module layout
- JWT and response-code contracts diverged enough that direct patch copying is unsafe

## Key Technical Lessons

### Repository history matters

A remote URL change is not the same thing as a clean codebase migration.

When repository ownership or organization changes mid-project, contribution strategy needs to be reset deliberately:
- active upstream
- legacy upstream
- personal fork
- portfolio repository

### Session design matters

JWT alone is not enough for stable sign-in behavior.

Refresh token storage strategy affects:
- logout behavior
- reissue behavior
- multi-device support
- incident response when a token is stolen or replayed

### API contracts must be verified end-to-end

A frontend that expects one refresh failure code and a backend that emits another will fail even when both sides look individually correct.

## Representative Contribution Themes

- Redis refresh token structure redesign
- multi-device refresh token support
- refresh-cookie behavior correction
- active/legacy server diff analysis
- contribution workflow reconstruction after repository split

## Suggested Evidence To Link

Add these after pushing the final work:
- PR links to `so-sim/server`
- PR links to any legacy backport work
- commit links from `jean202/sosim-server`
- screenshots or curl traces of refresh/login/logout behavior
- architecture notes or diagrams

## How To Present This In Interviews

Recommended framing:

1. The project had a broken repository lineage between legacy and active codebases.
2. I separated active contribution, legacy comparison, and portfolio documentation.
3. I improved JWT and Redis session handling instead of only patching symptoms.
4. I documented the migration and contract risks so future work could move faster.

## Repository Plan

This portfolio repository should contain:
- this README
- architecture comparison notes
- before/after diagrams
- PR index
- verification notes
- lessons learned

## Next Additions

- `docs/repo-lineage.md`
- `docs/jwt-redis-hardening.md`
- `docs/frontend-backend-contract-checklist.md`
- `docs/pr-index.md`
