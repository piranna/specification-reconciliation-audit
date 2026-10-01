---
name: specification-reconciliation-audit
license: MIT
description: >
  Perform a read-only, evidence-driven reconciliation between normative
  specifications, implementation, tests, operational documentation, persisted
  state/contracts, and equivalent implementations of the same invariant.
---

# Specification Reconciliation Audit

## Purpose

Perform a read-only, evidence-driven reconciliation between:

- normative specification;
- decision records and traceability material;
- implementation;
- tests;
- operational documentation;
- persisted state and serialized contracts;
- historical compatibility behavior;
- equivalent implementations of the same invariant.

The audit answers two different questions without conflating them:

1. What is the authoritative intended behavior?
2. What behavior is actually implemented, tested, documented, and persisted?

The audit must expose disagreement rather than silently reconcile it.

It is not an implementation pass and must not modify production code, tests,
state, refs, external systems, or normative documents unless a later task
explicitly authorizes remediation.

Read `references/audit-checklist.md` before beginning the audit.

## Core principles

### Evidence before interpretation

Every substantive finding must identify concrete evidence.

Prefer, in descending order of directness:

1. normative requirement or explicit accepted decision;
2. executable implementation;
3. tests demonstrating behavior;
4. persisted schemas, models, state transitions, or serialized contracts;
5. operational documentation;
6. historical compatibility behavior;
7. inference.

Do not convert inference into fact.

### Keep evidence classes separate

Use these labels when reasoning:

- `FACT` — directly demonstrated by inspected evidence;
- `INFERENCE` — conclusion derived from facts but not directly demonstrated;
- `RECOMMENDATION` — proposed action or design direction;
- `OPEN_QUESTION` — requires missing evidence or human authority.

A recommendation must never be presented as an existing requirement.

### Normative authority is explicit

Determine the repository's actual authority hierarchy before resolving
conflicts.

Do not assume that:

- tests define intended behavior;
- current implementation defines intended behavior;
- old documentation is normative;
- historical behavior is still required;
- newer code is automatically more authoritative.

When two sources disagree, record the disagreement and identify which source
has authority. If authority cannot be established, classify it as an open
question.

### Read-only means read-only

The audit may inspect repository contents, history, tests, state examples,
documentation, and read-only command output.

It must not:

- rewrite state;
- change branches or refs;
- perform recovery;
- publish anything;
- mutate external services;
- run commands whose nominal dry-run mode still has side effects;
- repair defects while discovering them.

If a probe is useful, first establish that it is isolated and non-mutating.
Prefer synthetic or temporary inputs.

### Coverage is not semantic completeness

Passing tests and 100% execution coverage are evidence, not proof that the
normative contract is completely implemented.

Look specifically for:

- missing state combinations;
- partial failures;
- negative cases;
- stale evidence;
- identity changes;
- concurrency and interruption;
- legacy states;
- equivalent operations with different validation strength;
- duplicated semantic implementations that have drifted.

## Audit modes

### Full audit

Use when:

- no trusted reconciliation baseline exists;
- the specification changed substantially;
- implementation changed across several semantic areas;
- a previous audit is stale or incomplete;
- the user explicitly requests a complete reconciliation.

Inspect all applicable surfaces and produce a complete finding inventory.

### Differential audit

Use when a trusted prior audit exists.

For each previous finding classify it as:

- `RESOLVED`;
- `STILL_PRESENT`;
- `CHANGED`;
- `REGRESSED`;
- `NO_LONGER_APPLICABLE`;
- `UNVERIFIABLE`.

Then search for `NEW` findings introduced since the baseline.

Do not merely rerun the previous audit and replace it. Preserve continuity and
explain why each changed classification changed.

Record:

- baseline document;
- baseline repository identity;
- current repository identity;
- relevant changed files or semantic areas.

## Required audit surfaces

Inspect every applicable surface below.

### Normative material

Identify:

- specification;
- accepted decision records;
- traceability matrices;
- open-question records;
- historical/superseded behavior records;
- explicit safety and validation policies.

Extract the normative invariants relevant to the audited scope.

### Implementation

Identify:

- entrypoints;
- command families;
- shared helpers;
- state transitions;
- persistence boundaries;
- external effects;
- validation and preflight checks;
- failure and recovery paths;
- dry-run paths;
- compatibility paths.

Do not limit inspection to the function named by the requirement. Follow the
operation through its complete semantic path.

### Tests

For each important behavior determine:

- which tests cover it;
- what those tests actually assert;
- fixtures and mocks that constrain the scenario;
- negative cases;
- partial-failure cases;
- idempotence cases;
- legacy-state cases;
- external effects that are mocked rather than demonstrated.

A test name is not evidence of what the test proves. Inspect its assertions.

### Persisted state and contracts

Inspect:

- models and schemas;
- serialized fields;
- identity/provenance fields;
- migration/default behavior;
- historical-state compatibility;
- transitions between states;
- evidence invalidation and supersession.

Check whether runtime behavior and persisted meaning agree.

### Operational documentation

Compare documentation with actual supported behavior.

Distinguish:

- normative rule;
- operator guidance;
- implementation description;
- historical note;
- example.

Documentation drift is a finding even when implementation is correct.

### Equivalent implementations

Search horizontally for operations that implement the same semantic invariant
through different helpers or command paths.

Examples include:

- identity validation;
- state persistence;
- branch/ref selection;
- operation detection;
- build or CI acceptance;
- recovery;
- archive/retirement;
- state discovery;
- dry-run/preflight handling.

Compare their strength, ordering, failure behavior, and evidence requirements.

Do not recommend unification merely to reduce duplication. Recommend it when
multiple implementations of the same semantic authority can diverge.

## Invariant matrix

Before producing findings, build a compact matrix for important invariants.

For each invariant record:

- normative source;
- implementation location(s);
- enforcing tests;
- persisted representation;
- operational documentation;
- observed disagreement;
- confidence.

This matrix is the main defense against auditing files independently and
missing cross-cutting semantic drift.

## Required semantic comparisons

For each applicable operation compare:

### Preconditions

- What must already be true?
- Where is it checked?
- Is the check authoritative or heuristic?
- Do equivalent paths perform the same check?

### Identity and evidence

- What object or generation does evidence belong to?
- Can evidence survive a changed commit, tree, generation, branch, or parent?
- Is stale evidence rejected, superseded, or silently reused?
- Can historical evidence be promoted to current truth without proof?

### Mutation ordering

Determine the order of:

1. validation/preflight;
2. local mutation;
3. persisted-state mutation;
4. external mutation;
5. verification;
6. final durable promotion.

Look for windows that can leave contradictory state after interruption.

### Failure semantics

Determine whether failures:

- fail closed;
- preserve recoverable evidence;
- partially persist;
- silently continue;
- infer success;
- require human authority.

### Dry-run semantics

A dry-run must be audited by behavior, not by flag name.

Check whether it can:

- checkout or change refs;
- fetch when fetch is considered mutation for the contract;
- write files/state/artifacts;
- create backups;
- notify;
- dispatch continuations;
- abort or alter active operations;
- invoke mutating helpers before reaching a dry-run guard.

### Recovery and resume

Check whether recovery:

- identifies the exact interrupted operation;
- validates current identity before resuming;
- distinguishes historical from current state;
- is idempotent;
- preserves provenance;
- can accidentally reinterpret a different checkout or generation.

## Findings and reporting

Before classifying or reporting findings, read
`references/finding-and-reporting.md`.

That reference defines:

- finding taxonomy;
- severity;
- current-path impact;
- required finding fields;
- report structure.

Severity and current-path impact are independent dimensions. A severe defect is
not automatically blocking to the current delivery path, and bounded technical
debt is not automatically a reason to expand the current task.

## Horizontal reconciliation

After local findings are collected, perform a second pass across semantic
families.

Ask:

- Is there already a stronger implementation elsewhere?
- Are two helpers enforcing the same invariant differently?
- Does one caller bypass a shared authority?
- Does one path persist evidence that another invalidates?
- Is one historical implementation weaker than its successor?
- Are tests freezing the weaker behavior?
- Could a single semantic authority remove the disagreement?

Report unification candidates only when they reduce semantic divergence, not
merely duplicated code.

## Open questions

An open question is required when:

- normative sources conflict without a defined authority;
- required intent cannot be inferred safely;
- historical compatibility may be deliberate;
- remediation would choose policy rather than implement existing policy.

State:

- the exact decision required;
- available options;
- evidence for each option;
- what remains blocked pending the decision.

Do not decide policy on behalf of the human authority.

## Output

Use the report structures defined in `references/finding-and-reporting.md`.

The report must preserve the distinction between evidence, interpretation,
recommendation, unresolved policy, severity, and current-path impact.

## Validation budget

When the audit needs executable validation evidence, use validation
proportionally rather than repeatedly rerunning the broadest available suite.

Prefer:

1. focused tests or probes while investigating a finding;
2. the repository's normal final/pre-commit validation at the appropriate
   completion boundary;
3. at most one complete final validation run unless a concrete new reason
   requires another.

Do not use the complete suite as an iterative debugging loop.

After non-behavioral changes, reuse still-valid evidence instead of repeating
expensive validation without a concrete reason.

Validation economy must never weaken required evidence. If a complete run is
normatively required, needed to establish trustworthy evidence, or required
because relevant behavior changed, perform it.

## Remediation ordering

Do not order remediation by file order or finding ID.

Prefer dependency and risk order:

1. safety/evidence correctness required by the current path;
2. authoritative semantic conflicts;
3. persistence and partial-failure correctness;
4. duplicated semantic authorities that can reintroduce defects;
5. missing tests needed to preserve corrected behavior;
6. documentation reconciliation;
7. performance and workflow debt;
8. historical cleanup not required by current operation.

The current delivery objective remains an explicit input to this ordering.

## Stop conditions

Stop and request human authority when:

- normative sources conflict and no authority rule resolves them;
- evidence is insufficient to distinguish defect from intentional behavior;
- the audit would require mutation to establish a conclusion;
- a proposed reconciliation changes policy rather than implements established
  policy.

Do not hide these cases to make the audit appear complete.

## Completion criteria

The audit is complete when:

- all applicable surfaces have been inspected;
- important invariants have end-to-end traceability;
- equivalent semantic implementations have been compared;
- every disagreement is classified;
- every finding has concrete evidence;
- current-path impact is explicit;
- open policy decisions are separated from engineering defects;
- deferred debt is bounded and documented;
- another engineer can reproduce the reasoning without relying on conversation
  history.
