# UiPath Integration Summary

UiPath has a practical developer stack for UIP-GH:

- `uip` CLI: scaffold, validate, push/pull, pack, publish, deploy, run, and file-level operations for agents. citeturn0search1turn0search0
- `uip mcp serve`: exposes the UiPath CLI as an MCP server so MCP clients can invoke UiPath commands. citeturn1search2
- SDKs: Python, LangGraph, LlamaIndex, MCP, and TypeScript options are available for coded development. citeturn1search0
- API Workflows: headless HTTP/connector-based system integration. citeturn1search5
- MCP Servers: UiPath-hosted, coded, command, and remote MCP servers can be used as Agent tools. citeturn1search9

Recommended UIP-GH layering:

```text
GitHub = Source of Truth
Agent  = Planning / Research / Code
UiPath = Execution / Enterprise Automation
MCP    = Agent ↔ UiPath bridge
CLI    = deterministic control surface
SDK    = custom extension
API    = system integration
```

The key finding is that UiPath does **not** need to replace GitHub. GitHub can remain the canonical source while `uip` CLI and MCP provide a controlled bridge into UiPath.

## Sources

- https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-agent
- https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-agent-push-pull
- https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-mcp
- https://docs.uipath.com/sdk/other/latest/developer-guide/using-agents-sdks
- https://docs.uipath.com/studio-web/automation-cloud/latest/user-guide/about-api-workflows
- https://docs.uipath.com/agents/automation-cloud/latest/user-guide/mcp-servers
