
## Using Your Claude Subscription LLMs

If you want Claude Code to use **your own Claude subscription** (and your own tokens) rather than Airia's, you need to update your configuration as follows.

> ⚠️ **Important:** Do **not** set `ANTHROPIC_API_KEY`. Leave it unset so Claude Code falls through to the subscription login flow and uses your "free" subscription tokens.

---

## Configuration Options

Choose **one** of the two methods below.

### Option 1 — `settings.json` (Recommended)

Edit the Claude settings file on your machine:

**File:** `~/.claude/settings.json`

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://<airia-gateway-url>",
    "ANTHROPIC_CUSTOM_HEADERS": "x-airia-key: <KEY_FROM_AIRIA_GATEWAY>"
  }
}
```

---

### Option 2 — Shell Environment Variables

Add the following to your `~/.zshrc` or `~/.bashrc`:

```bash
export ANTHROPIC_BASE_URL="https://<airia-gateway-url>"
export ANTHROPIC_CUSTOM_HEADERS="x-airia-key: <KEY_FROM_AIRIA_GATEWAY>"
```

Then reload your shell and launch Claude:

```bash
source ~/.zshrc   # or source ~/.bashrc
claude
```

---

## Summary

| | Option 1 (settings.json) | Option 2 (Shell vars) |
|---|---|---|
| **File to edit** | `~/.claude/settings.json` | `~/.zshrc` or `~/.bashrc` |
| **Persists across sessions?** | ✅ Yes | ✅ Yes |
| **Requires shell reload?** | ❌ No | ✅ Yes |
| **Recommended?** | ✅ Yes | Alternative |

> 💡 **Tip:** Do not set `ANTHROPIC_API_KEY` in either option — its absence is what triggers the Claude subscription login flow.
