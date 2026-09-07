# Claude Desktop via Airia Gateway

> **Claude Teams / Claude Enterprise**
>
> Requests forward the user's Anthropic OAuth token alongside the `x-airia-key` header — your Airia balance or customer Anthropic key is **not debited**.

---

## How It Works

Airia's AI Gateway supports a **User Impersonation** auth mode: instead of resolving requests against Airia's Anthropic key or a customer Anthropic key, the gateway forwards the user's own `Authorization: Bearer` token upstream to `api.anthropic.com`, alongside the `x-airia-key` header (which authenticates the request to Airia itself).

Two credentials are therefore in play — they must not be confused:

| Credential | Format | Where it goes | What it does |
|---|---|---|---|
| User's Anthropic OAuth token | `sk-ant-oat01-...` | Desktop → Gateway API key field → sent as `Authorization: Bearer` | Authenticates to Anthropic; bills the user's plan |
| Airia gateway key | Airia-issued format | Desktop → Custom inference header → `x-airia-key` | Authenticates to the Airia gateway / identifies the tenant config |

---

## Prerequisites

- Claude Desktop, recent version *(third-party inference UI is behind Developer Mode)*
- Claude Code CLI installed and logged in (`/login`) with the plan account *(Pro/Max/Team/Enterprise subscription auth — not an API key)*
- Airia AI Gateway configuration created with **AI Service Authentication = Use OAuth Passthrough**
  `Secure → Gateway → AI Gateway → Create New Configuration`
- The Airia gateway key (`x-airia-key` value) for that configuration
- The Airia gateway config's **Allowed Models** list must include the models you intend to use, plus `claude-haiku-4-5` *(Claude Desktop uses Haiku for background tasks such as thread naming)*. An empty list allows all models.

---

## Part 1 — Mint the User's Anthropic OAuth Token

> ⚠️ Run this on the **local machine** — not over plain SSH. The OAuth callback targets `localhost`.

**1.** In a terminal, run:

```bash
claude setup-token
```

**2.** A browser window opens to `claude.ai/oauth/authorize` (scope: `user:inference`, PKCE flow). Make sure the browser profile is signed in to the correct plan account, then approve.

**3.** The browser redirects to a callback URL containing `?code=...&state=...`. If the CLI doesn't capture it automatically, copy the `code` value and paste it into the waiting CLI prompt. *(Some versions expect `code#state`.)*

> **Note:** The code is a one-time, short-lived authorisation code — it is **not** the token. Only the CLI can exchange it, because it holds the PKCE verifier.

**4.** The CLI prints the token: `sk-ant-oat01-...`
This is a long-lived (~1 year) subscription-scoped token.

> 🔐 **Treat the `sk-ant-oat01-....` token like a password.** Anyone holding it can consume the user's plan quota. Do not commit it to source control. Do not share it in Slack. If leaked, log out/in via Claude Code and mint a new one.

---

## Part 2 — Configure Claude Desktop

**1.** Enable Developer Mode:
`Help → Troubleshooting → Enable Developer Mode`
*(The app restarts and a Developer menu appears.)*

**2.** Open `Developer → Configure Third-Party Inference…`

**3.** Navigate to `Connection → Gateway`

**4.** Enter gateway credentials:

| Field | Value |
|---|---|
| Gateway base URL | `https://prodaus.gateway.airia.ai` *(or your regional Airia gateway)* |
| Credential kind | `Static API key` |
| Gateway API key | The `sk-ant-oat01-...` token from Part 1 |
| Gateway auth scheme | `bearer` *(required — plan tokens travel as `Authorization: Bearer`, not `x-api-key`)* |
| Custom inference header | `x-airia-key` = the Airia-issued gateway key *(⚠️ not the Anthropic token — see failure modes below)* |

**5.** Under **Models**, add your primary models (e.g. `claude-sonnet-4-6`, an Opus model) and `claude-haiku-4-5`. These must match the Airia config's Allowed Models list.

**6.** Click **Test connection** — expect model discovery plus a short inference round-trip to succeed.

**7.** Click **Apply Changes**.

