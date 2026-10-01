# Finding and Reporting Reference

Use this reference after evidence collection, when classifying findings and
constructing the audit report.

## Finding taxonomy

Classify each finding primarily as one of:

- `NORMATIVE_CONFLICT`
- `IMPLEMENTATION_DEFECT`
- `TEST_SPEC_DISAGREEMENT`
- `SEMANTIC_DUPLICATION_DRIFT`
- `MISSING_SAFETY_PREFLIGHT`
- `EVIDENCE_IDENTITY_DEFECT`
- `STATE_CONTRACT_DRIFT`
- `DOCUMENTATION_DRIFT`
- `RECOVERY_OR_PARTIAL_FAILURE_RISK`
- `DRY_RUN_SIDE_EFFECT`
- `LEGACY_COMPATIBILITY_CONFLICT`
- `MISSING_TEST`
- `VALIDATION_WORKFLOW_DEBT`
- `PERFORMANCE_DEBT`
- `OPEN_QUESTION`

A finding may have secondary categories, but choose one primary category.

## Severity

Use:

### Critical

A demonstrated path can cause severe destructive or externally irreversible
behavior, or can silently establish materially false authoritative state with
no adequate recovery boundary.

### High

A demonstrated or strongly evidenced defect can:

- violate a normative safety invariant;
- accept invalid or stale evidence;
- mutate the wrong identity/generation/ref;
- lose required provenance;
- publish/retire/recover incorrectly;
- produce materially false success;
- make an imminent operation unsafe or unverifiable.

### Medium

A real semantic disagreement or missing guard exists, but exploitation requires
a narrower state, a human-visible failure remains, or the affected path is not
currently required.

### Low

Documentation drift, weak diagnostics, bounded maintainability problems, or
minor validation/test gaps with no demonstrated immediate correctness or safety
impact.

Severity describes the defect. It does not by itself decide whether current
delivery must stop.

## Blocking and current-path impact

For every finding separately record `current_path_impact`:

- `BLOCKING`
- `CONDITIONAL`
- `NON_BLOCKING`
- `UNKNOWN`

A finding is `BLOCKING` when the next required operation cannot be performed
safely, its result cannot be verified reliably, or proceeding would violate an
applicable invariant.

A finding may be deferred when all of the following are true:

- the risk is understood;
- its scope is bounded;
- the current operation does not depend on the defective behavior;
- required evidence remains trustworthy;
- safety invariants remain enforced;
- the debt is documented.

Unknown or unbounded risk must not be silently reclassified as technical debt.

Do not expand the current delivery task merely because unrelated debt was
discovered.

## Finding format

Every finding must contain:

- stable ID;
- title;
- primary category;
- severity;
- current-path impact;
- confidence;
- normative requirement or authority;
- observed implementation behavior;
- concrete evidence;
- affected implementation paths;
- relevant tests;
- persisted-state impact, when applicable;
- operational consequence;
- whether a probe demonstrated the behavior;
- recommended reconciliation direction;
- whether human authority is required.

Use file/function/test references precise enough for another engineer to verify
the claim.

Do not inflate finding count by reporting the same semantic defect once per
caller. Consolidate shared root causes and list affected paths.

## Output

A full audit should contain:

1. Snapshot and scope
2. Executive summary
3. Authority hierarchy
4. Normative invariant matrix
5. Findings summary
6. Detailed findings
7. Horizontal semantic reconciliation
8. Tests and validation gaps
9. Documentation/state drift
10. Open questions
11. Current-path risk assessment
12. Recommended remediation order
13. Deferred bounded debt
14. Evidence index

A differential audit should additionally contain:

1. Baseline identity
2. Resolved findings
3. Still-present findings
4. Changed findings
5. Regressions
6. New findings
7. Updated current-path risk assessment
