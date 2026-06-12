# ADR 0001: Adopt a Protocol-First Architecture

- Status: Accepted
- Date: 2026-06-13

## Context

Master needs to communicate with multiple Agent Runtimes without embedding
Runtime-specific lifecycle, commands, or evidence formats into Master Core.

The initial practical Runtime is OpenClaw, but future consumers may include
Codex, Claude Code, Cursor, Hermes, and other Runtimes.

## Decision

Agent Runtime Contracts will use a protocol-first architecture.

The repository will define:

- portable schemas;
- lifecycle semantics;
- evidence semantics;
- capability negotiation;
- conformance requirements;
- optional Runtime profiles.

It will not implement Runtime execution.

## Why Protocol-First

Protocol-first design allows:

- independent Master and Adapter evolution;
- language-neutral implementations;
- multiple Adapter implementations;
- mechanical compatibility testing;
- explicit unsupported capabilities;
- stable ownership boundaries.

## Why Not SDK-First

An SDK-first design would prematurely select:

- an implementation language;
- transport behavior;
- Runtime invocation patterns;
- error-handling abstractions;
- dependency and release policy.

An SDK may later implement the protocol, but it must not define the protocol.

## Why Not Derive the Protocol From OpenClaw Helpers

OpenClaw helpers encode implementation-specific assumptions, including:

- command behavior;
- filesystem layout;
- Agent lifecycle;
- local naming conventions;
- model selection;
- cleanup policy;
- native evidence formats.

Reverse-engineering the common protocol from those helpers would preserve
OpenClaw coupling rather than remove it.

## Why OpenClaw Is a Profile

OpenClaw is a useful first conformance target because it provides concrete
execution behavior.

It remains a profile because:

- other Runtimes expose different lifecycle primitives;
- core fields must be portable;
- unsupported capability reporting is preferable to forced emulation;
- no single Runtime should define the common abstraction.

## Consequences

Positive consequences:

- Master Core remains Runtime-neutral;
- Adapters can evolve independently;
- Runtime differences remain visible;
- conformance can be tested generically.

Costs:

- protocol semantics must be designed before implementation;
- alpha revisions are expected;
- some Runtime-native features will remain profile extensions;
- a future SDK must follow rather than redefine the protocol.
