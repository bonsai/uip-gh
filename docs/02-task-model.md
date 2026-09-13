# Task Model

## Task lifecycle

Every agent task follows:

`request → inspect → plan → execute → verify → report`

## Task contract

Each task should define:

- Goal: the desired outcome
- Scope: repositories, files, Issues, or external systems allowed
- Inputs: URLs, Issue numbers, files, or structured data
- Constraints: limits, exclusions, and safety rules
- Output: comment, commit, PR, report, or data file
- Verification: checks that prove completion

## Task classes

### Triage

Read Issues and classify them without changing project behavior.

### Research

Gather external information and produce cited, structured results.

### Data

Normalize, deduplicate, validate, and update machine-readable data.

### Review

Inspect a PR and report risks, regressions, missing tests, or documentation gaps.

### Maintenance

Update documentation, indexes, metadata, and other low-risk project artifacts.

### Automation

Operate external systems through APIs or UI automation when explicitly authorized.

## Default behavior

Start read-only. Make mutations only when the task explicitly permits them or the workflow defines an approved mutation step.