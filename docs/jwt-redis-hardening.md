# JWT/Redis Hardening

## Problem Statement

The refresh-token flow had to be improved across two constraints at the same time:

- the active server repository already had its own `/auth`-based JWT flow
- the older branch of work had already proven a stronger Redis/JWT design around multi-device refresh handling

The goal was not to copy the older branch blindly. The goal was to port the proven security and operability improvements into the active branch without discarding the active branch structure.

## Before

The active branch originally assumed a simpler refresh-token model:

- one refresh token per user
- Redis persistence tied to a single `userId`
- logout flow that removed cookies but did not necessarily invalidate the Redis token for the current device
- refresh flow centered on the refresh token value without a device context

That model is easier to start with, but it becomes weak when the product needs:
- multiple devices per user
- selective logout
- server-side token invalidation with clearer session identity

## After

The ported design introduced a multi-device refresh model.

### Redis shape

```text
KEY    = refresh:{userId}
FIELD  = deviceId
VALUE  = refreshToken
TTL    = 14 days
```

This makes it possible to:
- store multiple sessions for the same user
- invalidate one device without removing the entire account session state
- remove every refresh token for a user during withdrawal or forced logout flows

### JWT refresh behavior

The refresh token now carries the user id in the token subject.

That enables a validation sequence like this:

1. read `refreshToken` and `deviceId` from cookies
2. extract `userId` from the refresh token
3. load the stored refresh token from `refresh:{userId}` using `deviceId`
4. compare stored token and presented token
5. validate JWT signature and expiration
6. rotate and store a new refresh token for the same device

### Cookie behavior

The active branch now issues:
- refresh token cookie
- `deviceId` cookie

This keeps device identity explicit during refresh and logout flows.

## Why This Is Better

### 1. Session identity is clearer

The system no longer treats all refresh state for a user as one opaque login.

### 2. Logout is real invalidation

Cookie deletion alone is not a logout. The Redis entry for the current device also has to go away.

### 3. The design is operationally safer

It is easier to inspect all active device sessions for a user and easier to respond to suspicious session behavior.

### 4. Reviewability improved

Instead of shipping one large historical branch, the port was split into two reviewable commits:

- core JWT/Redis migration
- verification helpers and migration notes

## Reviewable Commit Breakdown

### Commit 1

[`cf67574`](https://github.com/jean202/so-sim-server/commit/cf67574)

Main scope:
- Redis hash storage
- device-aware refresh validation
- refresh token subject usage
- cookie issuance updates
- logout invalidation wiring

### Commit 2

[`bc2af17`](https://github.com/jean202/so-sim-server/commit/bc2af17)

Main scope:
- dev verification endpoints
- withdrawal cookie cleanup
- migration documentation

## Validation

The ported changes were validated with:

- `bash ./gradlew compileJava`
- `bash ./gradlew test`

## Remaining Improvement Opportunities

The migration improved the refresh flow substantially, but it does not close every auth-related gap.

Future candidates include:
- explicit detection and response for replayed or stolen refresh tokens
- stronger frontend/backend refresh error contract alignment
- review of access-token storage on the frontend
- access-token expiration policy review

## Related Documents

- [repo-lineage.md](./repo-lineage.md)
- [pr-index.md](./pr-index.md)
