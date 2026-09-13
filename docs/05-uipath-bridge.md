# UiPath Bridge

## Role

UiPath is the optional execution bridge for tasks that are easier through browser or enterprise application automation.

## Use UiPath when

- A website requires repeated UI interaction
- A legacy business application has no practical API
- Human-style browser operations are the main workload
- A workflow needs UiPath connectors or unattended execution

## Keep GitHub as the source of truth

The preferred architecture is:

`GitHub Issue → Agent planning → UiPath execution → evidence/result → GitHub`

UiPath should not become the only place where task definitions or important decisions exist.

## Boundary

The GitHub Agent decides what should happen. UiPath performs approved external-system actions. GitHub records the resulting artifact or evidence.

## Safety

Before external mutation, verify:

- target system
- target record
- intended change
- authorization
- expected result

For uncertain or destructive actions, stop and request human confirmation.