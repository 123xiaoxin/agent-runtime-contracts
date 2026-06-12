# Evidence Model

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

Observed evidence is produced from Runtime-observed execution activity.

Examples include:

- recorded tool invocations;
- tool results;
- lifecycle events;
- authorization pauses;
- created artifact references;
- Runtime errors;
- checkpoint creation;
- terminal Runtime state.

Observed trace is the source of truth for what occurred inside the Runtime.

Observed evidence does not, by itself, prove that the Master goal was achieved.

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

## Normalization Boundary

The common protocol must not require a Runtime's raw transcript, database
record, session JSON, or vendor event format.

Adapters normalize Runtime evidence into portable events and may provide
references to protected native evidence.

## Redaction

Sensitive evidence may be redacted, but the trace must disclose:

- that redaction occurred;
- which evidence category was affected;
- whether the audit is complete;
- whether the missing detail limits acceptance.

## Acceptance Boundary

Only the Master determines whether observed and audited evidence satisfies the
Execution Contract.

Runtime evidence supports acceptance. It does not replace acceptance.
