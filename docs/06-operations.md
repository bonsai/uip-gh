# Operations

## Standard loop

1. Receive a GitHub Issue or explicit task
2. Inspect the repository and relevant context
3. Create a short plan
4. Execute the smallest useful change
5. Verify the result
6. Publish evidence to GitHub

## Recommended artifacts

Use the smallest artifact that makes the work auditable:

- Comment for analysis or status
- Commit for a small direct change
- Pull request for reviewable implementation
- Data file for structured research output
- Documentation file for durable decisions

## Verification checklist

Before declaring success:

- Requested scope was respected
- Output exists at the expected path
- Data format is valid
- Links and references work when applicable
- Tests or checks were run when available
- No unrelated files were changed

## Cost discipline

Prefer existing GitHub capabilities and deterministic scripts before adding external services. Use UiPath only when its browser or enterprise automation capability provides a clear advantage.

## Agent etiquette

Be explicit about uncertainty. Never claim an external action succeeded without evidence. Keep commits focused and messages descriptive.

## Initial roadmap

Phase 1: Issue triage and repository inspection.

Phase 2: Research-to-JSON workflows.

Phase 3: PR review and automated documentation.

Phase 4: UiPath bridge for browser-based execution.

Phase 5: Event-driven orchestration and scheduled agents.