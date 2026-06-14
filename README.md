# Agent Runtime Contracts

Status: `1.0.0-alpha.2`

Agent Runtime Contracts is a runtime-neutral minimal protocol for execution
request, observed trace, Runtime result, and Master acceptance handoff.

## What This Repository Is

This repository defines:

- how a Master sends an opaque Execution Contract to a Runtime boundary;
- how a Runtime reports ordered observed events;
- how a Runtime reports its execution result;
- how events and result form a verifiable trace bundle;
- where Runtime completion ends and Master acceptance begins.

It is a protocol repository, not an SDK.

## What This Repository Does Not Do

This repository does not:

- execute tasks;
- invoke Runtime commands;
- provide a Runtime Adapter implementation;
- select models or tools;
- define Master governance policy;
- define a universal Agent industry standard;
- replace an Agent Runtime.

## Minimal Protocol Flow

```text
Execution Request
-> Observed Runtime Events
-> Execution Result
-> Trace Bundle
-> Master Acceptance Handoff
```

The trace records what the Runtime observed. The Master uses the trace,
artifacts, validation references, and its own Execution Contract to decide
acceptance.

## Component Boundary

| Component | Responsibility |
|---|---|
| Master Core | Owns intent governance, the Execution Contract, Runtime selection, and final acceptance |
| Agent Runtime Contracts | Defines the minimal request, event, result, trace, and handoff structures |
| Runtime Adapter | Translates protocol messages to and from one Runtime |
| Agent Runtime | Performs execution and emits Runtime-observed evidence |

## Execution Contract Ownership

The Master owns the Execution Contract.

This protocol carries the contract through an opaque envelope containing:

- schema URI;
- schema version;
- opaque payload.

The protocol validates the envelope but does not interpret the contract payload.

An envelope may also contain a digest. Digest support is retained as a future
integrity direction, but it is not required by the minimal `1.0.0-alpha.2`
implementation.

When an implementation opts into alpha digest conformance for a JSON payload,
it may use RFC 8785 canonical JSON with SHA-256. JSON Schema validates only the
digest field shape; recomputing the payload digest is an optional conformance
check.

## Observed Trace

The minimal trace contains:

- ordered Runtime events;
- artifact, validation, and evidence references;
- one Runtime execution result;
- stable request, trace, and Runtime identities.

Observed Runtime trace is the factual source for what occurred inside the
Runtime boundary. It does not decide whether the Master goal was achieved.

## Runtime Success Is Not Master Acceptance

A Runtime result of `succeeded` means the Runtime completed its execution path.

It does not mean:

- the Master accepted the deliverable;
- the Execution Contract was satisfied;
- the original user goal was achieved;
- the final status is `completed`.

Only the Master may perform final acceptance.

## Repository Scope

Protocol version `1.0.0-alpha.2` contains:

- adapter manifest and execution request schemas;
- Runtime event, execution result, and trace bundle schemas;
- generic success and acceptance-review examples;
- minimal structural and trace conformance rules.

SDKs, Runtime Adapters, control commands, checkpoints, Runtime hooks, and
transport implementations remain out of scope.

## Maturity

The protocol is experimental.

Consumers must negotiate exact alpha protocol versions and must not assume
compatibility between different alpha revisions.
