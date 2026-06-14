# Protocol Lifecycle

## Lifecycle Overview

```text
Handshake
-> Execution Request
-> Request Accepted
-> Observed Runtime Events
-> Execution Result
-> Trace Bundle
-> Master Acceptance Handoff
```

The minimal protocol defines portable evidence at the Runtime boundary. It does
not define an SDK, transport, Adapter implementation, or Master decision
engine.

## 1. Handshake

The Adapter publishes an `adapter_manifest` containing:

- Adapter and Runtime identity;
- supported protocol versions;
- supported capabilities;
- unsupported capabilities;
- limits;
- profile identity.

Unsupported capabilities must be declared explicitly.

For this stage, protocol version negotiation targets `1.0.0-alpha.2`.

## 2. Execution Request

The Master sends an `execution_request` containing:

- stable request and trace identity;
- an idempotency key;
- an opaque Master-owned `contractEnvelope`;
- requested capabilities;
- budget and authorization policy;
- event delivery preference.

The protocol validates the envelope structure but does not interpret its
payload.

## 3. Request Accepted

The Adapter validates:

- protocol compatibility;
- request structure;
- required capabilities;
- idempotency identity;
- authorization compatibility.

Acceptance means the request is valid and registered. It does not mean
execution has started.

Request acceptance is transport behavior in `1.0.0-alpha.2`; it is not a required
Runtime trace event.

## 4. Observed Runtime Events

The Runtime boundary emits ordered events with a shared `requestId`, `traceId`,
and `runtimeId`.

The minimal event types are:

| Event | Meaning |
|---|---|
| `execution_started` | Runtime execution began |
| `progress` | A non-terminal progress observation occurred |
| `artifact_produced` | The Runtime produced an artifact reference |
| `validation_started` | Validation began |
| `validation_completed` | Validation produced `passed`, `failed`, or `error` |
| `execution_completed` | Runtime execution completed operationally |
| `execution_failed` | Runtime execution failed |
| `execution_cancelled` | Runtime execution was cancelled |

Events are Runtime-observed records. They may contain artifact, validation, and
evidence references, but they do not contain Master acceptance decisions.

Sequence values must be unique, continuous, and increasing from `1`.
Timestamps must be non-decreasing.

Runtime-native events must be normalized before entering the common protocol.

## 5. Terminal Event

Each trace contains exactly one terminal event:

- `execution_completed`;
- `execution_failed`; or
- `execution_cancelled`.

The terminal event must be the final event in sequence.

## 6. Execution Result

The Runtime emits one `execution_result` summarizing its operational outcome:

- `succeeded`;
- `failed`; or
- `cancelled`.

The result includes:

- execution timestamps;
- event count;
- artifact, validation, and evidence references;
- a summary;
- a non-authoritative `masterAcceptanceHint`.

Result status must match the terminal event:

| Terminal event | Result status |
|---|---|
| `execution_completed` | `succeeded` |
| `execution_failed` | `failed` |
| `execution_cancelled` | `cancelled` |

`succeeded` means the Runtime execution path completed. It does not mean the
Master accepted the result.

## 7. Trace Bundle

The `trace_bundle` joins:

- the opaque `contractEnvelope`;
- ordered Runtime events;
- one execution result;
- stable request, trace, and Runtime identity.

The bundle provides enough portable references for the Master to inspect
artifacts, validation signals, and Runtime evidence. It does not embed or
require a vendor's raw session format.

## 8. Master Acceptance Handoff

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

`masterAcceptanceHint` is advisory only. It cannot mark the work accepted.

## Optional Alpha Digest Conformance

`contractEnvelope.digest` is optional in `1.0.0-alpha.2`.

Implementations are not required to canonicalize or hash a payload to implement
the minimal trace protocol. If digest conformance is enabled for a JSON payload,
the alpha convention is RFC 8785 canonical JSON encoded as UTF-8 and hashed
with SHA-256.

Digest verification does not authorize the protocol to interpret the opaque
Master Contract payload.

## Deferred Work

The minimal lifecycle does not define:

- control commands;
- resumable checkpoints;
- streaming transport mechanics;
- Runtime hooks;
- SDK or Adapter APIs.
