# Protocol Lifecycle

## Lifecycle Overview

```text
Handshake
-> Request Accepted
-> Execution Started
-> Progress / Event Stream
-> Authorization Required
-> Checkpoint
-> Result
-> Trace Bundle
-> Master Acceptance
```

Not every execution uses every optional stage.

## 1. Handshake

The Adapter publishes an `adapter_manifest` containing:

- Adapter and Runtime identity;
- supported protocol versions;
- supported capabilities;
- unsupported capabilities;
- limits;
- profile identity.

Unsupported capabilities must be declared explicitly.

## 2. Request Accepted

The Adapter validates:

- protocol compatibility;
- request structure;
- required capabilities;
- idempotency identity;
- authorization compatibility.

Acceptance means the request is valid and registered. It does not mean
execution has started.

## 3. Execution Started

The Runtime begins work associated with the accepted request and trace.

The Adapter must preserve the original `requestId`, `traceId`, and
`idempotencyKey`.

## 4. Progress and Event Stream

The Adapter exposes ordered execution events when supported.

Events may describe:

- progress;
- tool activity;
- artifacts;
- warnings;
- capability degradation;
- verification;
- blocking conditions.

Runtime-native events must be normalized before entering the common protocol.

## 5. Authorization Required

When an action exceeds current authorization, execution must pause before the
action occurs.

The Adapter reports:

- the requested capability;
- intended action;
- expected impact;
- whether execution can continue without it.

The Adapter must not grant itself additional authority.

## 6. Checkpoint

A Runtime may expose a resumable checkpoint.

A checkpoint identifies the protocol state and may carry an opaque
Runtime-specific resume token. The token is not interpreted by the core
protocol.

Checkpoint support is optional and must be declared during handshake.

## 7. Result

The Runtime emits one terminal execution result:

- `succeeded`;
- `partial`;
- `blocked`;
- `failed`;
- `cancelled`.

These are Runtime execution outcomes, not Master acceptance outcomes.

## 8. Trace Bundle

A trace bundle contains normalized Runtime-observed evidence associated with
the request.

The trace bundle may reference Runtime-native evidence without embedding a raw
vendor format into the core schema.

## 9. Master Acceptance

The Master evaluates:

- the original Execution Contract;
- Runtime result;
- artifacts;
- verification evidence;
- authorization history;
- trace audit;
- remaining risks.

The Master then decides whether the goal is accepted, partially accepted,
rejected, or requires further execution.

## Contract Envelope Digest

For JSON contract payloads in `v1-alpha.1`, the digest is:

1. canonicalize the payload using RFC 8785;
2. encode it as UTF-8;
3. compute SHA-256;
4. store the lowercase hexadecimal value.

The protocol treats the payload as opaque despite verifying its digest.
