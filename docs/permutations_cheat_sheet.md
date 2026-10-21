# Permutations
  
  
## Claude Code

### Config Paths

- **Mac/Linux:** `~/.claude/settings.json`
- **Windows:** `%USERPROFILE%\.claude\settings.json`

### Airia Key

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://prodaus.gateway.airia.ai",
    "ANTHROPIC_API_KEY": "<AIRIA_GATEWAY_KEY>"
  }
}
```

### Auth Passthrough

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://prodaus.gateway.airia.ai",
    "ANTHROPIC_CUSTOM_HEADERS": "x-airia-key: <AIRIA_GATEWAY_KEY>"
  }
}
```

### OTel

Uses the same `settings.json` as above. These keys go in the same `env` block as the gateway config:

```json
{
  "env": {
    "CLAUDE_CODE_ENABLE_TELEMETRY": "1",
    "OTEL_EXPORTER_OTLP_PROTOCOL": "http/json",
    "OTEL_EXPORTER_OTLP_HEADERS": "X-API-Key=<AIRIA_OTEL_KEY>",
    "OTEL_EXPORTER_OTLP_LOGS_ENDPOINT": "https://prodaus.api.airia.ai/v1/ClaudeCodeOtelIngest/ingest",
    "OTEL_LOG_USER_PROMPTS": "1",
    "OTEL_LOG_TOOL_DETAILS": "1",
    "OTEL_LOG_RAW_API_BODIES": "1",
    "OTEL_LOGS_EXPORTER": "otlp",
    "OTEL_RESOURCE_ATTRIBUTES": "airia.user-email=<USER_EMAIL>"
  }
}
```

### MCP Servers

MCP servers are stored separately from `settings.json`:

- **Mac/Linux:** `~/.claude.json`
- **Windows:** `%USERPROFILE%\.claude.json`

---

## Claude Desktop

### Config Paths

- **Mac:** `~/Library/Application Support/Claude-3p/configLibrary/_meta.json`
- **Windows:** `%LOCALAPPDATA%\Claude-3p\configLibrary\_meta.json`
- **Alternative Windows Registry Pathway:** `HKCU\SOFTWARE\Policies\Claude\inferenceGatewayBaseUrl`

### `_meta.json`

Points to a `<GUID>.json` file:

```json
{
  "appliedId": "<GUID>",
  "entries": [
    {
      "id": "<GUID>",
      "name": "Default"
    }
  ]
}
```

### Actual `<GUID>.json` We Modify

The gateway and OTel settings both live in this one file. They're shown separately below, but they go in the same JSON object.

#### Gateway: Airia Key Auth

```json
{
  "inferenceGatewayBaseUrl": "https://prodaus.gateway.airia.ai/anthropic",
  "inferenceGatewayApiKey": "<AIRIA_GATEWAY_KEY>",
  "inferenceGatewayAuthScheme": "x-api-key",
  "chatTabEnabled": true,
  "disableDeploymentModeChooser": true,
  "inferenceProvider": "gateway",
  "inferenceCredentialKind": "static"
}
```

#### Gateway: Passthrough with Minted Key

```json
{
  "inferenceGatewayBaseUrl": "https://prodaus.gateway.airia.ai/anthropic",
  "inferenceCustomHeaders": {
    "x-api-key": "<AIRIA_GATEWAY_KEY>"
  },
  "inferenceGatewayApiKey": "<MINTED_ANTHROPIC_KEY>",
  "chatTabEnabled": true,
  "disableDeploymentModeChooser": true,
  "inferenceProvider": "gateway",
  "inferenceCredentialKind": "static"
}
```

#### OTel

The same for both gateway options:

```json
{
  "otlpEndpoint": "https://prodaus.api.airia.ai/v1/ClaudeCodeOtelIngest/ingest",
  "otlpProtocol": "http/json",
  "otlpHeaders": {
    "X-API-Key": "<AIRIA_OTEL_KEY>"
  },
  "otlpResourceAttributes": {
    "airia.user-email": "<USER_EMAIL>"
  },
  "otlpDesktopLogLevel": "info",
  "otlpContentCapture": [
    "userPrompts",
    "toolDetails",
    "rawApiBodies"
  ]
}
```

### MCP Servers

#### Config Paths

- **Mac:** `~/Library/Application Support/Claude-3p/claude_desktop_config.json`
- **Windows:** TBD

#### `claude_desktop_config.json`

Each MCP server is connected through the Airia MCP Gateway using `mcp-remote`:

```json
{
  "mcpServers": {
    "<MCP_SERVER_NAME>": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://prodaus.mcp-gateway.airia.ai/gateway/<GATEWAY_ID>/radar"
      ]
    },
    "<MCP_SERVER_NAME_2>": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://prodaus.mcp-gateway.airia.ai/gateway/<GATEWAY_ID_2>/mcp"
      ]
    }
  }
}
```

---

## Codex / ChatGPT

### Config Paths

- **Mac:** `~/.codex/config.toml`
- **Windows:** `%USERPROFILE%\.codex\config.toml`

Mac and Windows use the same TOML config.

### Airia Key Usage

```toml
model = "gpt-5.6-sol"
model_provider = "airia"
model_reasoning_effort = "medium"

[model_providers.airia]
name = "Airia AI Gateway"
base_url = "https://prodaus.gateway.airia.ai/openai/v1"
env_key = "AIRIA_API_KEY"
wire_api = "responses"
requires_openai_auth = false
```

### Passthrough Auth

```toml
model_provider = "airia"

# --- Airia Gateway (begin) ---
[model_providers.airia]
name = "Airia AI Gateway"
base_url = "https://prodaus.gateway.airia.ai/openai/v1"
wire_api = "responses"
requires_openai_auth = true
http_headers = { "x-airia-key" = "<AIRIA_GATEWAY_KEY>" }
```

### MCP Servers

MCP servers are added to the same `config.toml` (Mac: `~/.codex/config.toml`) and connect directly to the Airia MCP Gateway using OAuth:

```toml
[mcp_servers.<MCP_SERVER_NAME>]
url = "https://prodaus.mcp-gateway.airia.ai/gateway/<GATEWAY_ID>/mcp"
auth = "oauth"
```
