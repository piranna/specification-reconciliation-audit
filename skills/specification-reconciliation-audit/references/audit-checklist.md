# Specification Reconciliation Audit Checklist

Use this checklist as a coverage guard, not as a substitute for reasoning.

Mark an item `N/A` only with a short explanation.

## 1. Snapshot and scope

- [ ] Record repository/worktree identity.
- [ ] Record branch/ref where relevant.
- [ ] Record whether the worktree is clean or modified.
- [ ] Record the normative/specification version or files audited.
- [ ] Record the implementation baseline.
- [ ] Record the test evidence baseline.
- [ ] Record the previous audit baseline for differential mode.
- [ ] Define included semantic areas.
- [ ] Define explicit exclusions.
- [ ] Confirm the audit itself is read-only.

## 2. Authority

- [ ] Identify the normative specification.
- [ ] Identify accepted decision records.
- [ ] Identify traceability material.
- [ ] Identify open-question records.
- [ ] Identify historical/superseded behavior records.
- [ ] Establish the authority hierarchy.
- [ ] Record unresolved authority conflicts instead of guessing.

## 3. Normative invariants

For each important invariant:

- [ ] Requirement identified.
- [ ] Normative source recorded.
- [ ] Implementation location recorded.
- [ ] Tests recorded.
- [ ] Persisted representation recorded.
- [ ] Operational documentation recorded.
- [ ] Agreement/disagreement classified.
- [ ] Confidence recorded.

## 4. Entrypoints and command families

For each applicable entrypoint:

- [ ] Preconditions identified.
- [ ] Branch/ref/identity assumptions identified.
- [ ] State inputs identified.
- [ ] External effects identified.
- [ ] Persistence points identified.
- [ ] Validation gates identified.
- [ ] Failure behavior identified.
- [ ] Resume/recovery behavior identified.
- [ ] Dry-run behavior identified.
- [ ] Legacy behavior identified.

## 5. State and persistence

- [ ] Models agree with normative semantics.
- [ ] Serialization preserves required identity/provenance.
- [ ] Defaults do not invent successful evidence.
- [ ] Migrations preserve or explicitly supersede historical meaning.
- [ ] State transitions are legal and explicit.
- [ ] Failed operations cannot silently promote state.
- [ ] Partial operations have identifiable recovery state.
- [ ] Repeated execution is idempotent where required.
- [ ] Historical state cannot silently become current truth.
- [ ] Read-only/historical state cannot be mutated accidentally.

## 6. Identity and evidence

Check applicable identities:

- [ ] Commit SHA.
- [ ] Tree SHA.
- [ ] Parent/base identity.
- [ ] Branch/ref identity.
- [ ] Generation identity.
- [ ] External revision identity.
- [ ] CI/Try revision identity.
- [ ] Validation-group identity.
- [ ] Persisted evidence ownership.

For each:

- [ ] Evidence is tied to the correct identity.
- [ ] Changed identity invalidates evidence when required.
- [ ] Historical evidence is not silently reassigned.
- [ ] Strong equivalence proof is required where identity changes.
- [ ] Evidence provenance survives normalization/reconciliation.

## 7. Validation and acceptance

- [ ] Required validation matrix is explicitly defined.
- [ ] Observed success is not confused with required coverage.
- [ ] Missing jobs/checks cannot become success silently.
- [ ] Equivalent consumers use compatible acceptance rules.
- [ ] Manual success cannot be synthesized without authority/evidence.
- [ ] Build evidence belongs to the correct contents/generation.
- [ ] Test evidence belongs to the correct contents/generation.
- [ ] Stale validation is rejected or explicitly historical.

## 8. Mutation ordering and atomicity

For each mutating workflow:

- [ ] Preconditions occur before mutation.
- [ ] Local mutation order is known.
- [ ] State persistence order is known.
- [ ] External mutation order is known.
- [ ] Verification occurs before durable success.
- [ ] Interruption between each stage has defined semantics.
- [ ] Rollback/recovery does not assume unverified success.
- [ ] Archive/recovery evidence exists before destructive retirement.
- [ ] Lease/concurrency protection is applied where required.

## 9. Dry-run

For every command advertising dry-run:

- [ ] No branch checkout.
- [ ] No ref mutation.
- [ ] No state write.
- [ ] No artifact write unless explicitly part of the contract.
- [ ] No backup creation unless explicitly part of the contract.
- [ ] No external publication.
- [ ] No notification unless explicitly part of the contract.
- [ ] No continuation/abort dispatch.
- [ ] No mutating helper executes before the dry-run guard.
- [ ] Any permitted observation/fetch behavior is explicitly documented.

## 10. Recovery and resume

- [ ] Interrupted operation is uniquely identifiable.
- [ ] Current Git/state identity is revalidated.
- [ ] Recovery does not infer success from stale state.
- [ ] Recovery is idempotent.
- [ ] Recovery preserves provenance.
- [ ] Recovery distinguishes current from historical snapshots.
- [ ] Partial external success is handled explicitly.
- [ ] Equivalent recovery paths enforce the same invariants.

## 11. Tests

For each important invariant:

- [ ] Positive case exists.
- [ ] Negative case exists.
- [ ] Ambiguous case fails closed.
- [ ] Partial-failure case exists.
- [ ] Idempotence/re-entry case exists where applicable.
- [ ] Legacy-state case exists where applicable.
- [ ] Identity-change case exists where applicable.
- [ ] External effects are distinguished from mocks.
- [ ] Assertions prove the behavior implied by the test name.
- [ ] 100% execution coverage is not treated as semantic completeness.

## 12. Equivalent semantic implementations

Search horizontally for duplicated authorities involving:

- [ ] Identity validation.
- [ ] Branch/ref selection.
- [ ] Git-operation detection.
- [ ] State persistence.
- [ ] CI/build/test acceptance.
- [ ] Recovery.
- [ ] Archive/retirement.
- [ ] State discovery.
- [ ] Dry-run/preflight behavior.
- [ ] Historical reconciliation.

For each family:

- [ ] Implementations compared.
- [ ] Strongest semantics identified.
- [ ] Weaker/historical variants identified.
- [ ] Different failure behavior identified.
- [ ] Different persistence behavior identified.
- [ ] Different evidence requirements identified.
- [ ] Tests enforcing divergent behavior identified.
- [ ] Unification recommended only if it removes semantic divergence.

## 13. Documentation

- [ ] Operational docs match supported behavior.
- [ ] Normative rules are distinguishable from examples.
- [ ] Historical behavior is labelled historical.
- [ ] Deprecated behavior is not presented as current.
- [ ] Recovery instructions match implementation.
- [ ] Dry-run claims match implementation.
- [ ] Validation requirements match implementation.
- [ ] State-field meanings match models and runtime use.

## 14. Finding quality

For every finding:

- [ ] Stable ID.
- [ ] Concise title.
- [ ] Primary category.
- [ ] Severity.
- [ ] Current-path impact.
- [ ] Confidence.
- [ ] Normative authority.
- [ ] Observed behavior.
- [ ] Concrete file/function/test evidence.
- [ ] Persisted-state impact where applicable.
- [ ] Operational consequence.
- [ ] Probe result where applicable.
- [ ] Recommended reconciliation direction.
- [ ] Human decision requirement stated.
- [ ] Root cause consolidated instead of duplicated per caller.

## 15. Severity sanity check

### Critical

- [ ] Severe destructive/irreversible path or materially false authoritative
      state is demonstrated.

### High

- [ ] Safety invariant, evidence correctness, identity, provenance,
      publication/retirement/recovery, or materially false success is affected.

### Medium

- [ ] Real semantic defect exists but is narrower, visible, or outside the
      immediate required path.

### Low

- [ ] Primarily documentation, diagnostics, maintainability, bounded test gap,
      or workflow/performance debt.

## 16. Current-path impact

For every finding:

- [ ] `BLOCKING`, `CONDITIONAL`, `NON_BLOCKING`, or `UNKNOWN` assigned.
- [ ] Classification justified independently from severity.
- [ ] Next required operation identified when relevant.
- [ ] Evidence reliability assessed.
- [ ] Safety invariant impact assessed.
- [ ] Unknown/unbounded risk not relabelled as debt.

A finding may be deferred only when:

- [ ] Risk is understood.
- [ ] Scope is bounded.
- [ ] Current operation does not depend on defective behavior.
- [ ] Required evidence remains trustworthy.
- [ ] Safety invariants remain enforced.
- [ ] Debt is documented.

## 17. Differential audit

When a baseline audit exists:

- [ ] Baseline repository identity recorded.
- [ ] Current repository identity recorded.
- [ ] Each old finding classified:
  - [ ] `RESOLVED`
  - [ ] `STILL_PRESENT`
  - [ ] `CHANGED`
  - [ ] `REGRESSED`
  - [ ] `NO_LONGER_APPLICABLE`
  - [ ] `UNVERIFIABLE`
- [ ] New findings explicitly marked `NEW`.
- [ ] Changed classifications include evidence.
- [ ] Previously accepted debt is rechecked against the current path.
- [ ] Regression search includes fixes that introduced weaker parallel paths.

## 18. Open questions

For every unresolved policy issue:

- [ ] Exact decision stated.
- [ ] Competing authoritative evidence stated.
- [ ] Options stated neutrally.
- [ ] Consequences of each option stated.
- [ ] Blocking effect stated.
- [ ] No policy invented by the auditor.

## 19. Validation budget

When executable validation is required:

- [ ] Focused tests/probes are preferred during investigation.
- [ ] The complete suite is not used as an iterative debugging loop.
- [ ] Normal final/pre-commit validation is used at the appropriate completion
      boundary.
- [ ] At most one complete final validation run is budgeted unless a concrete
      new reason requires another.
- [ ] Still-valid evidence is reused after non-behavioral changes.
- [ ] Validation economy does not weaken normatively required evidence.

## 20. Remediation ordering

Confirm remediation is ordered by dependency/risk:

- [ ] Current-path safety/evidence blockers.
- [ ] Normative semantic conflicts.
- [ ] Persistence/partial-failure correctness.
- [ ] Divergent semantic authorities.
- [ ] Tests required to preserve corrected behavior.
- [ ] Documentation reconciliation.
- [ ] Performance/validation-workflow debt.
- [ ] Deferred historical cleanup.

Do not order merely by finding ID or file location.

## 21. Final audit quality gate

Before declaring the audit complete:

- [ ] Every applicable surface was inspected.
- [ ] Important invariants have end-to-end traceability.
- [ ] Horizontal comparison was performed.
- [ ] Facts, inferences, recommendations, and open questions are separated.
- [ ] Every finding has reproducible evidence.
- [ ] Severity and current-path impact are separate.
- [ ] Deferred debt is bounded.
- [ ] Unknown risk remains explicitly unknown.
- [ ] No mutation was performed as part of the audit.
- [ ] Another engineer can reproduce the conclusions without conversation
      history.
