![Airia Code](/images/heading.png)
  
# Airia Configuration Guides — Claude Suite

This repository contains setup guides for connecting the Claude Suite to the Airia Gateway.

## Guides

### Airia Gateway Setup
Step-by-step instructions for setting up an AI Gateway in Airia
- [Airia Gateway Setup](docs/gateway/ai_gateway_setup.md)
- [MCP Gateway Setup](docs/gateway/mcp_gateway_setup.md)

### Claude Desktop (Claude CoWork / Claude Code)
Step-by-step instructions for connecting Claude Desktop to the Airia Gateway.

- [Claude Desktop Setup](docs/claude_desktop/claude_desktop_setup.md)
- [MCP Server Setup](docs/claude_desktop/mcp_server_setup.md)

### Claude Excel Add-In
Step-by-step instructions for connecting the Claude Excel Add-In to the Airia Gateway.

- [Claude Excel Setup](docs/claude_add_ins/claude_excel_setup.md)

### Claude Code CLI
Step-by-step instructions for connecting Claude Code CLI to the Airia Gateway.

- [Claude Code Setup](docs/claude_code/claude_code_setup.md)
  - [Mac/Linux](docs/claude_code/claude_code_setup.md#maclinux-configuration)
  - [Windows](docs/claude_code/claude_code_setup.md#windows-configuration)
  - [Trouble Shooting](docs/claude_code/claude_code_setup.md#troubleshooting)
- [MCP Server Setup](docs/claude_code/mcp_server_setup.md)
- [OTEL / Activity Monitoring Setup](docs/claude_code/OTEL_setup.md)
- [Bedrock Setup](docs/claude_code/bedrock_setup.md)


If you want Claude Code to use your own Claude subscription (and your own tokens) rather than Airia's, you need to update your configuration as follows.
- [Claude Subscription LLMs](docs/claude_code/claude_subscription_llm.md)

## Helpful Claude Scripts

### CLI

**Version**
```bash
claude --version
```

**Location**
```bash
which claude
```

### Config Files

**Claude Code settings**
```bash
cat ~/.claude/settings.json
```

**MCP servers** (look under the `mcpServers` key — this is a large file)
```bash
cat ~/.claude.json
```

### Cowork MCP

**View MCP servers configured for Claude CoWork**
```bash
cat ~/Library/Application\ Support/Claude-3p/claude_desktop_config.json
```

## Videos
The following video shows the code in action.

[![Alt text](https://img.youtube.com/vi/sEd_7Bz_VKs/0.jpg)](https://youtu.be/sEd_7Bz_VKs)
