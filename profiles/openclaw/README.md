# OpenClaw Reference Profile

OpenClaw is the first reference profile for Agent Runtime Contracts.

It is not the default Runtime and does not define core protocol semantics.

## Profile Scope

This profile may document:

- capability mappings;
- unsupported capabilities;
- Runtime limits;
- lifecycle mapping constraints;
- evidence normalization rules;
- known compatibility limitations.

## Adapter Boundary

The actual OpenClaw Adapter will belong in a separate repository:

```text
agent-runtime-adapter-openclaw
```

This profile must not contain executable Adapter code.

## Prohibited Core Leakage

The following must not enter core schemas:

- OpenClaw commands;
- OpenClaw installation or workspace paths;
- helper script interfaces;
- temporary Agent naming rules;
- Agent Pack behavior;
- OpenClaw model routing;
- raw session JSONL structure;
- OpenClaw-specific cleanup behavior.

Where native OpenClaw evidence must be retained, the Adapter should expose a
portable normalized event and an optional protected reference to the native
evidence.
