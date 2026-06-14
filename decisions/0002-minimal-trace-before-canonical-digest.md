# ADR 0002: Implement Minimal Trace Before Canonical Digest

- Status: Accepted
- Date: 2026-06-15

## Context

The first protocol skeleton established Runtime-neutral boundaries, an
Execution Request, capability negotiation, and an opaque Master Contract
envelope.

It did not yet define the core evidence path:

```text
Runtime Event -> Execution Result -> Trace Bundle -> Master Acceptance Handoff
```

At the same time, `contractEnvelope.digest` was required and the documentation
required RFC 8785 canonical JSON with SHA-256. That made envelope integrity a
minimum implementation gate before the protocol could express a basic Runtime
trace.

## Decision

Protocol version `1.0.0-alpha.2` will prioritize a minimal verifiable trace.

This stage adds:

- `runtime_event.v1`;
- `execution_result.v1`;
- `trace_bundle.v1`;
- generic success and acceptance-review examples;
- minimal trace consistency checks.

`contractEnvelope.digest` becomes optional.

The protocol continues to:

- treat the Master Contract payload as opaque;
- retain the digest definition for optional use;
- keep RFC 8785 plus SHA-256 as an optional alpha conformance path;
- separate Runtime success from Master acceptance.

## Why Trace Comes First

The protocol must first prove that it can:

- preserve execution identity;
- record ordered Runtime observations;
- reference artifacts and validation evidence;
- produce one coherent Runtime result;
- hand evidence back to the Master for acceptance.

Without that path, stronger digest rules protect an envelope that the protocol
cannot yet connect to a verifiable execution outcome.

## Why Canonical Digest Is Optional

Canonical digest verification may be valuable for integrity, replay, caching,
or audit.

It is not required to demonstrate the minimal execution-to-acceptance handoff.
Making it optional:

- reduces the implementation threshold;
- avoids forcing every early implementation to support RFC 8785;
- preserves the long-term direction without blocking trace experiments;
- allows evidence from real implementations to shape later integrity rules.

## Why No SDK or Adapter Yet

An SDK would select language and API conventions before the trace semantics are
stable.

An Adapter implementation would risk encoding one Runtime's native behavior
into the core protocol.

Stage 2 therefore defines schemas, examples, and conformance semantics only.

## Consequences

Positive:

- the protocol gains an end-to-end evidence path;
- digest-free implementations can conform to the minimal alpha;
- Runtime success and Master acceptance become testable as separate concepts;
- Runtime-specific fields remain outside core schemas.

Costs:

- digest-present and digest-absent paths both need examples or tests;
- alpha consumers must negotiate `1.0.0-alpha.2`;
- transport, control, checkpoint, SDK, and Adapter questions remain deferred.
