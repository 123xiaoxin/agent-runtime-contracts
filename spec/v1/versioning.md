# Versioning

## Protocol Version

Protocol versions use Semantic Versioning with prerelease identifiers.

The current protocol version is:

```text
1.0.0-alpha.2
```

Alpha versions must be negotiated exactly. Support for `alpha.1` does not imply
support for `alpha.2`.

`1.0.0-alpha.2` introduces the minimal Runtime event, execution result, and
trace bundle schemas. It also makes `contractEnvelope.digest` optional.

## v1-alpha Compatibility Policy

During alpha:

- documentation clarifications may occur without a protocol bump;
- any validation-shape or semantic change requires a new alpha revision;
- consumers must advertise every supported protocol version;
- Adapters must reject unsupported required protocol versions;
- no silent compatibility assumption is allowed.

## Schema Versioning

Each schema has:

- a major namespace in its path;
- a repository-relative `$id`;
- a revisioned `schemaVersion` value.

Example:

```text
File: schemas/v1/execution_request.v1.schema.json
$id: ./execution_request.v1.schema.json
schemaVersion: execution_request.v1-alpha.2
```

A schema validation shape must not change without changing its revisioned
`schemaVersion`. The repository-relative `$id` remains stable within the major
schema path so that relative `$ref` resolution works offline.

Unchanged schemas may retain an earlier schema revision while advertising
support for the newer protocol version. In `1.0.0-alpha.2`,
`adapter_manifest.v1-alpha.1` and `common_defs.v1-alpha.1` remain unchanged,
while `execution_request` and the new trace schemas use `v1-alpha.2`.

After stable v1, a breaking schema change requires a v2 schema.

## Profile Versioning

Profiles are versioned independently from the core protocol.

A profile must declare:

- profile name;
- profile version;
- supported protocol versions;
- capability mappings;
- known limitations.

Updating a profile does not automatically change the core protocol.

## Breaking Changes

The following are breaking changes:

- removing a field;
- renaming a field;
- changing a field type;
- making an optional field required;
- narrowing an accepted value;
- changing lifecycle ordering;
- changing status meaning;
- changing digest interpretation;
- changing authorization semantics;
- changing the meaning of a capability identifier.

Adding an optional field is still incompatible with strict older receivers
unless a new negotiated schema revision is used.

Making a formerly required field optional is an alpha semantic change and
requires a new alpha protocol and schema revision.

## Non-Breaking Changes

The following are normally non-breaking:

- editorial clarification;
- new examples;
- new conformance cases for existing rules;
- a new independent profile;
- additional unsupported-capability explanations;
- implementation guidance that does not alter protocol meaning.

## Stable v1 Gate

Stable v1 should not be published until:

- the minimal request, event, result, and trace flow has been exercised;
- Runtime result and Master acceptance remain demonstrably separate;
- observed trace consistency checks are stable;
- optional digest behavior has implementation evidence.

Stable v1 does not require an SDK or a universal Agent protocol.

## Optional Digest Direction

Digest support remains part of the long-term envelope integrity direction.

For `1.0.0-alpha.2`:

- `contractEnvelope.digest` is optional;
- missing digest does not make a request or trace non-conformant;
- JSON Schema validates only the digest shape when present;
- RFC 8785 plus SHA-256 verification is an optional alpha conformance check;
- enabling digest verification does not permit payload interpretation.
