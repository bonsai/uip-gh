# UiPath MCP / API / SDK / CLI 調査結果

調査日: 2026-09-13

## 結論

**あります。しかも、UIP-GHとの相性はかなり良いです。**

UiPathは現在、単なるStudioのGUI製品ではなく、少なくとも次の4つの開発入口を持っています。

| 入口 | 役割 | UIP-GHでの位置づけ |
|---|---|---|
| `uip` CLI | Agentの作成・検証・push/pull・pack・publish・deploy・run | ⭐ 第一候補 |
| MCP | AI Agentから`uip` CLIをToolとして呼ぶ | ⭐ AI接続 |
| SDK | Python / LangGraph / LlamaIndex / MCP / TypeScript | ⭐ 本格実装 |
| API Workflows / Orchestrator API | HTTP/APIによるシステム連携 | ⭐ 外部システム連携 |

## 1. UiPath CLI

現在のCLIは `uip` です。

低コードAgentについては、以下のライフサイクルをCLIだけで扱えます。

```text
uip agent init
      ↓
uip agent validate
      ↓
uip agent push
      ↓
Studio Web
      ↓
uip agent pull
      ↓
uip agent pack
      ↓
uip agent publish
      ↓
uip agent deploy
      ↓
uip agent run
```

`uip agent push` はローカルのAgentプロジェクトをStudio Webへ送信し、`pull` はStudio Webから`.uis`として取得できます。`.uis`はAgentプロジェクトのZIP形式の transport format です。

さらに `uip agent file` では、プロジェクト全体をpull/pushせず、Studio Web内の個別ファイルをlist/get/putできます。

### UIP-GHとの意味

GitHubをsource of truthにするなら、次の構成が可能です。

```text
GitHub
  │
  ├── agent.json
  ├── entry-points.json
  ├── evals/
  └── docs/
       │
       ▼
     uip CLI
       │
       ▼
  UiPath Studio Web
```

つまり、**UiPath GUIを本体にしなくても、GitHub → CLI → UiPathという開発ルートを作れる**。

## 2. MCP

UiPath CLIには `uip mcp serve` があります。

これは `uip` CLI自体をMCP Serverとして公開する機能です。MCP対応AgentからUiPath CLIのサブコマンドをToolとして呼び出せます。

```text
AI Agent
   │
   │ MCP
   ▼
 uip mcp serve
   │
   ▼
 uip agent / codedagent / ...
   │
   ▼
UiPath Platform
```

公式ドキュメントではClaude Desktop、Claude Code、Cursor、VS CodeなどのMCPホストから利用する例が示されています。

重要なのは、MCP側のToolが巨大な専用APIではなく、`run_command`としてCLIコマンドを実行する設計になっている点です。

そのため、Agentには原則としてJSON出力を要求するのがよいです。

```text
uip agent list --output json
uip agent validate ./my-agent --output json
uip agent push ./my-agent --output json
```

### 注意

`uip mcp serve` は認証済みCLIセッションをそのまま利用します。また、MCP経由で破壊的なCLI操作も実行できるため、**GitHub Agentから無制限に使わせる設計は避ける**べきです。

UIP-GHでは、IssueやAgentのTask Contractで許可する操作を限定する設計が適しています。

## 3. SDK

UiPathはCoded Agent向けに複数のSDKを提供しています。

- Python SDK
- Python + LangGraph SDK
- LlamaIndex SDK
- MCP SDK
- TypeScript SDK

特にMCP SDKは、PythonでMCP Serverを構築・実行するためのSDKです。

また、UiPathのMCP Server基盤では `uipath-mcp-python` がランタイム / SDK / CLIを提供し、`uipath-python`をベースにしています。

### UIP-GHとの意味

「UiPathを呼ぶAgent」と「UiPath上で動くAgent」を分離できます。

```text
                    ┌── GitHub Agent
                    │
UIP-GH Controller ──┼── UiPath CLI
                    │
                    ├── UiPath MCP
                    │
                    └── Custom MCP Server
```

これにより、UiPathを単なるRPA実行環境ではなく、**Agent Tool群を提供する実行基盤**として扱えます。

## 4. API / API Workflows

UiPath Studio WebにはAPI Workflowsがあります。

HTTP endpointやIntegration Service connectorを組み合わせて、UIを使わないsystem-to-system連携を構築できます。

```text
GitHub webhook
      ↓
API Workflow
      ↓
UiPath / external API
      ↓
JSON result
      ↓
GitHub
```

GitHub Issue、Webhook、外部サービス、データ処理などをつなぐ用途に向いています。

Orchestrator APIもあり、Job開始、Queue、Asset、Job Output、Robot StatusなどをAPIから操作できます。

## 5. Coded Agent

Coded AgentはPython中心のコード開発にも対応しています。

Studio Webはcontrol plane、IDEはprimary development environmentという位置づけです。

したがって、GitHubを開発本拠地にする思想と矛盾しません。

```text
GitHub
  ↓
IDE / Coding Agent
  ↓
Python Coded Agent
  ↓
uip codedagent push
  ↓
Studio Web
  ↓
Orchestrator
```

## 6. UIP-GHでの推奨構成

現時点では、次の順番が一番きれいです。

### Level 1 — GitHub Agent

通常のコード・Issue・PR・ResearchはGitHub側で完結。

### Level 2 — UiPath CLI

UiPath Agentのbuild / validate / push / pull / publish / deployをCLIから操作。

### Level 3 — UiPath MCP

GitHub Agentから必要なUiPath CLI操作をMCP Toolとして呼ぶ。

### Level 4 — UiPath API

API WorkflowやOrchestrator APIで外部サービスとの連携を追加。

### Level 5 — Custom MCP / SDK

UiPathにない専門ToolをPython SDKなどで作る。

```text
                 UIP-GH
                    │
          ┌─────────┴─────────┐
          │                   │
      GitHub Agent        UiPath MCP
          │                   │
          │                uip CLI
          │                   │
          │          ┌────────┴────────┐
          │          │                 │
       GitHub      Agent/API       Orchestrator
          │          │                 │
          └──────────┴─────────────────┘
```

## 7. Research Agentにやらせるべき最初の仕事

今回の調査結果から、UIP-GHのResearch Agentには次を定義できる。

> **「UiPath公式ドキュメントを調査し、MCP / API / SDK / CLIの機能を比較して、GitHub上の設計文書としてcommitする」**

つまり、今回やったこと自体をAgentの標準Taskにできます。

```text
Issue
  ↓
Research Agent
  ↓
official docs search
  ↓
compare MCP / API / SDK / CLI
  ↓
structured findings
  ↓
Markdown
  ↓
GitHub commit
```

## 8. 最終判断

**UiPathをGitHubの代わりにする必要はない。**

むしろ、

```text
GitHub = Source of Truth
Agent  = Planning / Research / Code
UiPath = Execution / Enterprise Automation
MCP    = Agent ↔ UiPath bridge
CLI    = deterministic control surface
SDK    = custom extension
API    = system integration
```

という分業が最も自然です。

特に `uip mcp serve` + `uip agent ...` があることで、UIP-GHの「GitHubを頭脳にしてUiPathを手足にする」という当初設計は、かなり現実的になっています。

## Sources

- UiPath CLI — `uip agent`: https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-agent
- UiPath CLI — `uip agent push / pull`: https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-agent-push-pull
- UiPath CLI — `uip mcp`: https://docs.uipath.com/uipath-cli/standalone/latest/user-guide/uip-mcp
- UiPath Agents SDKs: https://docs.uipath.com/sdk/other/latest/developer-guide/using-agents-sdks
- UiPath MCP Server foundation: https://docs.uipath.com/orchestrator/automation-cloud/latest/user-guide/mcp-server-shared-foundation
- UiPath API Workflows: https://docs.uipath.com/studio-web/automation-cloud/latest/user-guide/about-api-workflows
- UiPath Agents — MCP Servers: https://docs.uipath.com/agents/automation-cloud/latest/user-guide/mcp-servers
