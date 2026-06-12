# Scope and Boundaries

## Master Core

The Master Core is responsible for:

- interpreting user intent;
- defining the real goal and non-goals;
- producing and owning the Execution Contract;
- defining allowed and forbidden actions;
- selecting required capabilities;
- setting authorization and budget policy;
- selecting a Runtime route;
- evaluating Runtime evidence;
- deciding final acceptance.

The Master Core does not delegate acceptance authority to the Runtime.

## Agent Runtime Contracts

This repository is responsible for:

- versioned protocol envelopes;
- capability negotiation;
- portable lifecycle semantics;
- execution request structure;
- evidence classification;
- Runtime status semantics;
- conformance requirements;
- profile extension boundaries.

It does not execute a request or interpret a Master-owned contract payload.

## Runtime Adapter

A Runtime Adapter is responsible for:

- advertising truthful capabilities and limits;
- translating a protocol request into Runtime-native operations;
- preserving request, trace, and idempotency identity;
- translating Runtime events into protocol events;
- enforcing authorization boundaries;
- exposing Runtime-observed evidence;
- reporting unsupported capabilities without simulation;
- preserving Runtime-specific resume data as opaque data.

An Adapter must not silently strengthen permissions or claim capabilities the
Runtime cannot provide.

## Agent Runtime

An Agent Runtime is responsible for:

- managing its execution session;
- invoking its own tools and models;
- producing artifacts;
- pausing when authorization is required;
- exposing available progress and execution evidence;
- reporting execution outcomes and failures.

A Runtime does not determine whether the Master goal was achieved.

## Dependency Direction

```text
Master Core --------\
                     -> Agent Runtime Contracts
Runtime Adapter ----/

Runtime Adapter -> Agent Runtime
```

Agent Runtime Contracts must not depend on Master implementation code, Adapter
implementation code, or a specific Agent Runtime.

## Prohibited Core Content

The following must not enter the common contract:

- Runtime-native command names;
- Runtime installation paths;
- Runtime session storage formats;
- raw vendor event or transcript formats;
- specific model or provider names;
- prompt templates or skills;
- expert libraries;
- fixed Agent-count policies;
- temporary Agent naming conventions;
- Master phase names;
- Runtime-specific cleanup rules;
- Git workflow policy;
- product UI behavior;
- credentials or secret values;
- OpenClaw helper behavior;
- OpenClaw session JSONL structure.

Runtime-specific information belongs in a versioned profile or Adapter
implementation.

## Extension Rule

Vendor-specific extensions must:

- be optional;
- be namespaced;
- remain outside required core semantics;
- not change the meaning of core fields;
- not be required for generic conformance.
