# Setting Up Claude Desktop with Airia

## Prerequisites
- An Airia account
- Claude Desktop installed

---

## 1. Get Your Gateway API Key

1. In Airia, navigate to **Gateway → AI Gateway**
2. Locate your Claude Desktop gateway (e.g. `Claude_Desktop`) and copy the API key — it starts with `agk-`

---

## 2. Configure Claude Desktop

1. Open Claude Desktop and go to **Help → Troubleshooting → Enable Developer Settings**
2. Navigate to **Developer → Open Third-Party Inference → Connection**
3. Enter the following:

| Field | Value |
|---|---|
| **Configuration** | Gateway |
| **Gateway Base URL** | `https://prodaus.gateway.airia.ai/anthropic` *(Australian tenant)* |
| **Gateway API Key** | Your `agk-` key from Step 1 |
| **Gateway Auth Scheme** | `x-api-key` |

4. Click **Apply Locally**

---

## 3. Verify the Connection

In the bottom-left of Claude Desktop, you should see **Cowork 3P · Gateway** — 
this confirms the gateway is active.

Click **New Task**, type a prompt, and press Enter. A successful response confirms 
your setup is complete.