
# Configuring Claude Code for Airia Gateway + AWS Bedrock

## 1. Configure Your Airia Gateway

Before updating your local settings, make sure your Airia gateway has AWS Bedrock enabled and the relevant models allowed.

1. In Airia, navigate to **Gateway > AI Gateway** and open the gateway you want to use
2. Go to the **General** tab and scroll to **AI providers**
3. Enable the **AWS Bedrock** toggle and select your AWS Bedrock connection

![AWS Bedrock enabled in AI Gateway](/images/bedrock_gateway_enabled.png)

4. Under **Allowed Models**, select the foundation models you want Claude Code to use

![Allowed models configuration](/images/bedrock_allowed_models.png)

5. Click **Save**


## 2. Open Your Claude Settings File

- **Mac/Linux:** `~/.claude/settings.json`
- **Windows:** `%USERPROFILE%\.claude\settings.json`

## 3. Add the Bedrock Configuration

Merge the following `env` block into your settings file:

```json
{
  "env": {
    "CLAUDE_CODE_ENABLE_AUTO_MODE": "1",
    "CLAUDE_CODE_USE_BEDROCK": "1",
    "ANTHROPIC_BEDROCK_BASE_URL": "https://prodaus.gateway.airia.ai",
    "CLAUDE_CODE_SKIP_BEDROCK_AUTH": "1",
    "ANTHROPIC_API_KEY": "<your-gateway-api-key>",
    "AWS_REGION": "ap-southeast-2",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "au.anthropic.claude-haiku-4-5-20251001-v1:0",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "au.anthropic.claude-sonnet-4-6",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "au.anthropic.claude-opus-4-8",
    "ANTHROPIC_DEFAULT_FABLE_MODEL": "au.anthropic.claude-fable-5"
  }
}
```

Replace `<your-gateway-api-key>` with your Airia Gateway API key (`agk-...`).

## 4. Notes

- **Don't set `ANTHROPIC_BASE_URL`** (wrong protocol, breaks gateway requests). Use `ANTHROPIC_BEDROCK_BASE_URL` only; don't append `/bedrock`.
- **`CLAUDE_CODE_USE_BEDROCK=1`** sends Bedrock-native requests (`/model/{modelId}/invoke`), which the gateway expects.
- **`CLAUDE_CODE_SKIP_BEDROCK_AUTH=1`** bypasses local AWS credential signing; the gateway handles auth server-side.
- **`ANTHROPIC_API_KEY`** is your gateway credential, sent as `x-api-key`.
- Bedrock inference profiles require a regional prefix on all model IDs (e.g. au., apac., global.) as bare model IDs won't work.
- All pinned models must be enabled in both your **Airia Gateway provider allow-list** and your **AWS Bedrock Marketplace** account.
