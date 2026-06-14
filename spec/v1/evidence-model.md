# Evidence Model

The minimal trace protocol separates claims, Runtime observations, and later
audit conclusions. This separation lets the Master evaluate evidence without
giving the Runtime authority over acceptance.

## Declared Evidence

Declared evidence is what a protocol participant claims occurred.

Examples include:

- Adapter progress reports;
- model-generated summaries;
- declared tool activity;
- declared artifacts;
- declared verification results;
- terminal result descriptions.

Declared evidence is useful but is not authoritative by itself.

## Observed Evidence

Observed evidence is produced from Runtime-observed execution activity and
normalized into `runtime_event` records.

Examples include:

- execution start and terminal state;
- progress observations;
- created artifact references;
- validation references and outcomes;
- Runtime failure or cancellation observations;
- other supporting material linked through `evidenceRefs`.

Observed trace is the source of truth for what occurred inside the Runtime.

Observed evidence does not, by itself, prove that the Master goal was achieved.

## Evidence References

Runtime events and results may carry:

- `artifactRefs` for produced outputs;
- `validationRefs` for validation records;
- `evidenceRefs` for other observed supporting material.

References identify evidence without requiring the core protocol to embed
vendor-native transcripts or storage records.

The trace bundle must preserve enough references for the Master to locate the
evidence used for acceptance review.

## Execution Result

The execution result is a Runtime summary derived from the observed event
stream. It is declared evidence and must remain consistent with:

- the terminal event;
- event count;
- request, trace, and Runtime identity;
- referenced artifacts and validations.

A `succeeded` result is an operational statement, not an acceptance statement.
`masterAcceptanceHint` is non-authoritative.

## Audited Evidence

Audited evidence is the result of comparing declared evidence with observed
evidence.

An audit may detect:

- missing declared events;
- invented events;
- incorrect event order;
- identity mismatches;
- capability contradictions;
- missing authorization;
- artifact mismatches;
- result and trace inconsistencies.

## Authority Order

For Runtime activity:

```text
Audited Evidence
  evaluates
Declared Evidence <-> Observed Evidence
```

When declared and observed evidence conflict, observed evidence determines what
the Runtime actually did.

The audit determines whether the declared account is trustworthy.

## Trace Bundle

The trace bundle is the portable handoff unit from Runtime execution to Master
acceptance review.

It contains:

- the opaque Master Contract envelope;
- ordered observed Runtime events;
- the Runtime execution result;
- stable identity and creation time.

The trace bundle is not a Master acceptance record.

## Normalization Boundary

The common protocol must not require a Runtime's raw transcript, database
record, session JSON, or vendor event format.

Adapters normalize Runtime evidence into portable events and may provide
references to protected native evidence.

## Acceptance Boundary

Only the Master determines whether observed and audited evidence satisfies the
Execution Contract.

Runtime evidence supports acceptance. It does not replace acceptance.

A valid trace may therefore contain:

- `result.status = succeeded`;
- failed validation evidence;
- `masterAcceptanceHint = requires_review`;
- a later Master decision to reject or request repair.
