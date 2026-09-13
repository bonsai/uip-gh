# UIP-GH 🤖

**GitHub-first agent architecture for development, research, and automation.**

UIP-GH treats GitHub as the system of record and lets agents turn Issues into small, traceable, verifiable tasks. UiPath is an optional execution layer for browser and enterprise-system operations where direct APIs or GitHub-native automation are not enough.

## 🧭 Philosophy

```text
GitHub = home 🏠
Agent   = brain 🧠
UiPath  = hands 🦾
```

The core principle is simple:

> **Keep the intent, plan, evidence, changes, and results in GitHub.**

Agents should inspect first, make the smallest useful change, verify it, and report what happened.

## 🏗️ Architecture

```text
GitHub Issue
     │
     ▼
Agent: inspect → plan
     │
     ├── GitHub-native work
     │      ├── files
     │      ├── Issues
     │      ├── PRs
     │      └── reviews
     │
     └── UiPath bridge (when needed)
            │
            ▼
       browser / enterprise system
            │
            ▼
       evidence / result
            │
            ▼
          GitHub
```

## 📚 Documentation

| Doc | Purpose |
|---|---|
| [`docs/01-mission.md`](docs/01-mission.md) | Mission, principles, and scope |
| [`docs/02-task-model.md`](docs/02-task-model.md) | Common task contract and lifecycle |
| [`docs/03-github-agent.md`](docs/03-github-agent.md) | GitHub Agent responsibilities and workflow |
| [`docs/04-research-agent.md`](docs/04-research-agent.md) | Research, evidence, and structured data |
| [`docs/05-uipath-bridge.md`](docs/05-uipath-bridge.md) | UiPath as an optional execution bridge |
| [`docs/06-operations.md`](docs/06-operations.md) | Operational rules, verification, and roadmap |

## 🎯 First-class tasks

- **Issue triage** — classify, prioritize, detect duplicates, and propose next actions.
- **Repository health** — inspect structure, stale work, broken references, and missing documentation.
- **Research → data** — research a question, preserve evidence, normalize results, and commit structured data.
- **PR review** — inspect changes, identify risks, and report actionable findings.
- **Documentation maintenance** — keep README/docs aligned with the actual repository.
- **External automation** — use UiPath when browser UI or legacy systems are the practical interface.

## 🔄 Standard task loop

```text
request
  ↓
inspect
  ↓
plan
  ↓
execute
  ↓
verify
  ↓
report
```

Default behavior is **read-only first**. Mutating actions should be explicit, scoped, and reversible where possible.

## 🔐 Safety rules

- Do not silently merge, delete, force-push, or rewrite history.
- Prefer small changes and pull requests for meaningful mutations.
- Preserve source URLs and evidence for research tasks.
- Never present an inference as a verified fact.
- Before external-system mutation, confirm the target, intended change, authorization, and expected result.
- Keep GitHub as the canonical record even when UiPath performs the action.

## 💸 Cost discipline

Use GitHub-native operations and deterministic scripts first. Bring in UiPath or other external services only when they provide a clear advantage, such as UI-only workflows, legacy applications, or business-system integrations.

## 🚀 Roadmap

1. Issue triage and repository inspection
2. Research → structured JSON/JSONL
3. PR review and documentation maintenance
4. UiPath execution bridge
5. Event-driven and scheduled agent orchestration

## 📜 Status

This repository is the design and operating specification for the UIP-GH approach. Implementation can grow incrementally from these contracts; the architecture does not require UiPath to be present.
