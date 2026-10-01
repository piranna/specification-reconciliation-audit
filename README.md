# Specification Reconciliation Audit

A reusable Agent Skill for performing read-only, evidence-driven reconciliation
between normative specifications, implementation, tests, operational
documentation, persisted state/contracts, and equivalent implementations of
the same invariant.

The skill identifies semantic disagreement without silently inventing policy or
mutating the audited project.

## Skill

`specification-reconciliation-audit`

## Installation

The canonical source is this GitHub repository.

### skills CLI

Install globally for supported agents:

```bash
npx skills add piranna/specification-reconciliation-audit \
  --skill specification-reconciliation-audit \
  --global
```

The CLI can select the target agents interactively. Agent-specific targets can
also be supplied explicitly.

### GitHub CLI

For Codex:

```bash
gh skill install piranna/specification-reconciliation-audit \
  specification-reconciliation-audit \
  --agent codex \
  --scope user
```

Use `gh skill preview` before installing third-party skills.

## Usage

Ask the agent explicitly to use `specification-reconciliation-audit` and specify
whether a full or differential reconciliation audit is required.

Example:

> Use the specification-reconciliation-audit skill to perform a full,
> read-only reconciliation audit of this repository against its normative
> specification.

The audit itself is read-only. Remediation is a separate activity.

## Structure

- `skills/specification-reconciliation-audit/SKILL.md` — core audit methodology.
- `skills/specification-reconciliation-audit/references/audit-checklist.md` —
  detailed coverage checklist.
- `skills/specification-reconciliation-audit/references/finding-and-reporting.md`
  — finding taxonomy, severity and report format.
- `scripts/validate` — local structural and Agent Skills validation.

## Development

Changes to the methodology should preserve the distinction between:

- normative authority;
- observed evidence;
- inference;
- recommendations;
- unresolved policy questions;
- severity;
- current-path blocking impact.

The audit also follows a validation budget: focused validation during
investigation, normal final validation at the completion boundary, and no
repeated complete-suite runs without a concrete reason.

Run:

```bash
./scripts/validate
```

before committing a release.
