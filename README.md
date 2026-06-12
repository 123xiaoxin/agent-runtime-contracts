# Agent Runtime Contracts

Status: `v0.1-alpha`

Agent Runtime Contracts is a versioned protocol and conformance repository for
communication between a Master Core and Agent Runtimes.

## What This Repository Is

This repository defines:

- the boundary between Master Core, Runtime Adapter, and Agent Runtime;
- protocol messages and lifecycle semantics;
- capability negotiation;
- evidence and acceptance semantics;
- conformance expectations.

It is a protocol repository, not an SDK.

## What This Repository Does Not Do

This repository does not:

- execute tasks;
- invoke Runtime commands;
- provide a Runtime Adapter implementation;
- select models or tools;
- define Master governance policy;
- replace an Agent Runtime.

## Component Boundary

| Component | Responsibility |
|---|---|
| Master Core | Owns intent governance, the Execution Contract, Runtime selection, and final acceptance |
| Agent Runtime Contracts | Defines portable protocol messages, lifecycle semantics, and conformance rules |
| Runtime Adapter | Translates protocol messages to and from one Runtime |
| Agent Runtime | Performs execution and emits Runtime-observed evidence |

## Execution Contract Ownership

The Master owns the Execution Contract.

This protocol carries the contract through an opaque envelope containing:

- schema URI;
- schema version;
- digest;
- opaque payload.

The protocol validates the envelope but does not interpret the contract payload.

For JSON payloads, the alpha specification computes the digest from the
RFC 8785 canonical representation using SHA-256.

JSON Schema validates only the shape of the digest field. Verifying that the
digest matches `contractEnvelope.payload` is a conformance check, not a schema
check.

## Runtime Success Is Not Master Acceptance

A Runtime result of `succeeded` means the Runtime completed its execution path.

It does not mean:

- the Master accepted the deliverable;
- the Execution Contract was satisfied;
- the original user goal was achieved;
- the final status is `completed`.

Only the Master may perform final acceptance.

## Repository Scope

The `v0.1-alpha` skeleton contains:

- core boundary documents;
- lifecycle and evidence definitions;
- versioning rules;
- adapter manifest and execution request schemas;
- generic examples;
- an OpenClaw reference profile placeholder;
- conformance scope;
- architecture decisions.

Runtime event, checkpoint, result, control command, and trace bundle schemas are
deferred until the lifecycle semantics are reviewed.

## Maturity

The protocol is experimental.

Consumers must negotiate exact alpha protocol versions and must not assume
compatibility between different alpha revisions.
