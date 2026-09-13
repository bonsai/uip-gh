# UIP-GH Mission

## Purpose

UIP-GH is a GitHub-first agent system for turning repetitive development and research work into traceable tasks.

The source of truth is GitHub. Agents read Issues, inspect repositories, perform bounded work, and return evidence as comments, commits, or pull requests.

## Principles

- GitHub is the system of record
- Agent actions must be observable
- Prefer small reversible tasks
- Research must produce structured evidence
- Human approval is required for destructive or high-impact actions
- Do not hide decisions inside opaque automation

## First-class tasks

1. Issue triage
2. Repository health checks
3. Research and data collection
4. Data normalization
5. Pull request review
6. Documentation maintenance

## UiPath boundary

UiPath is an optional execution layer for browser and business-system automation. It is not the canonical project store.

## Success

A task is successful when another human or agent can inspect what was requested, what was done, and what evidence supports the result.