# Conformance

Conformance verifies the minimal request, observed trace, Runtime result, and
Master acceptance handoff structures.

## Minimal Checks

Stage 2 requires only the following checks:

1. Every JSON document parses.
2. Every schema passes JSON Schema Draft 2020-12 schema validation.
3. Every example validates against its corresponding schema.
4. Handshake protocol declarations use `1.0.0-alpha.2`, and each document
   matches its declared schema revision.
5. `requestId`, `traceId`, and `runtimeId` are consistent across a trace bundle.
6. Event sequence values are unique, continuous, and increasing from `1`.
7. Event timestamps are non-decreasing.
8. A trace contains exactly one terminal event, and it is the final event.
9. `result.eventCount` equals `events.length`.
10. `result.status` matches the terminal event:
    - `execution_completed` -> `succeeded`
    - `execution_failed` -> `failed`
    - `execution_cancelled` -> `cancelled`

Validators SHOULD enable JSON Schema format assertion for `date-time`, `uri`,
and `uri-reference` when available.

## Optional Digest Check

`contractEnvelope.digest` is optional for `1.0.0-alpha.2`.

When digest validation is explicitly enabled for a JSON payload, the alpha
convention is RFC 8785 canonical JSON with SHA-256. JSON Schema checks only the
digest field shape; recomputing and comparing the payload digest is an optional
semantic check.

## Conformance Principle

Passing JSON Schema validation alone does not establish conformance.

The minimal semantic checks above establish whether a trace is internally
consistent. They do not decide Master acceptance.
