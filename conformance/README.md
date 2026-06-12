# Conformance

Conformance verifies that an Adapter implements protocol semantics truthfully.

## v0.1-alpha Skeleton Scope

The first skeleton defines conformance categories but does not provide a
conformance runner.

The included generic examples must validate against their corresponding
schemas.

## Schema and Semantic Checks

JSON Schema validates the structure of protocol documents.

It does not establish every semantic invariant:

- `uniqueItems` compares complete array values and cannot guarantee that
  `capabilityId` is unique across objects with different descriptions or other
  fields. Capability ID deduplication is a conformance check.
- JSON Schema validates only the shape of a digest. Recomputing and comparing
  the digest of `contractEnvelope.payload` is a conformance check.
- Conformance validators SHOULD enable JSON Schema format assertion for
  `date-time`, `uri`, and `uri-reference` fields when the validator supports
  it.

## Planned Conformance Categories

- schema validation;
- JSON Schema format assertion;
- exact protocol version negotiation;
- capability declaration honesty;
- capability ID uniqueness;
- unsupported capability handling;
- request identity preservation;
- idempotency behavior;
- contract payload digest verification;
- lifecycle ordering;
- authorization pause behavior;
- terminal result uniqueness;
- declared versus observed evidence consistency;
- profile isolation;
- absence of vendor-specific core requirements.

## Conformance Principle

Passing JSON Schema validation alone does not establish conformance.

An Adapter must also satisfy lifecycle, authorization, evidence, and
truthfulness requirements.

## Profile Conformance

A profile may add tests for its Runtime mapping, but profile tests must not
change common protocol meaning or become mandatory for other Runtimes.
