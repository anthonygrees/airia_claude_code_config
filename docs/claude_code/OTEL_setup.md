# Setting Up OTEL Data to Send to Airia

---

## 1. Generate an Airia API Key

1. In Airia, navigate to **Discover → Connect → Claude**
2. Under **Services to Secure**, click **Claude Code**
3. Generate an **Airia API Key**

---

## 2. Setup via Script (macOS)

This is the easiest way to get up and running — no manual file editing required!

1. Click the **Download macOS Script** button to download `airia_otel_setup_macos.sh`
2. In your terminal, navigate to the folder where the script was saved:

```bash
cd ~/Downloads
```

3. Grant the script execution permission:

```bash
chmod +x airia_otel_setup_macos.sh
```

4. Run the script:

```bash
./airia_otel_setup_macos.sh
```

> **Note:** The script safely merges settings into your `~/.claude/settings.json` without overwriting anything that's already there. If you're already proxying Claude Code traffic through the Airia Gateway, those settings will be preserved!

The script will add the following configuration to your settings file:

```json
{
  "env": {
    "CLAUDE_CODE_ENABLE_TELEMETRY": "1",
    "OTEL_EXPORTER_OTLP_PROTOCOL": "http/json",
    "OTEL_EXPORTER_OTLP_HEADERS": "X-API-Key=ak-Airia-Key",
    "OTEL_EXPORTER_OTLP_LOGS_ENDPOINT": "https://prodaus.api.airia.ai/v1/ClaudeCodeOtelIngest/ingest",
    "OTEL_LOG_USER_PROMPTS": "1",
    "OTEL_LOG_TOOL_DETAILS": "1",
    "OTEL_LOG_RAW_API_BODIES": "1",
    "OTEL_LOGS_EXPORTER": "otlp",
    "OTEL_RESOURCE_ATTRIBUTES": "airia.user-email=<RESOLVED_AT_INSTALL>"
  }
}
```

---

## 3. Manual Setup

Prefer to do it yourself? No problem — just open `~/.claude/settings.json` and add the fields from the JSON block above directly.

---

## 4. View Your Claude Code Activity

Once set up, all telemetry from your Claude Code sessions flows into a centralised feed in Airia. Navigate to **Audit → Activity → Claude Monitoring** to see:

- **Sessions** — a full log of every Claude Code session
- **Prompts** — every prompt sent during those sessions
- **Raw Events** — the low-level OTEL event stream from Claude Code traffic

This gives your team complete visibility over how Claude Code is being used across the organisation, all in one place.
