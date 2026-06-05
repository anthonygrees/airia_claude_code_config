# Setting Up Claude Excel Add-In with Airia

## Prerequisites
- An Airia account
- Microsoft Excel

---

## 1. Get Your Gateway API Key

1. In Airia, navigate to **Gateway → AI Gateway**
2. Locate your AI gateway and copy the API key — it starts with `agk-`

![Airia Code](/images/gateway_api_key.png)
---
## 2. Install the Add-In

1. Open Excel and navigate to the **Home** tab
2. Click **Add-ins**
3. Search for and install the **Claude for Microsoft 365** add-in

---

## 3. Configure the Claude Excel Add-In

1. Click the **Claude** icon in Excel to open the add-in

![Open Claude Add-In](/images/excel_open_claude.png)

2. Click **Cloud provider or gateway**
3. Select **Gateway**
4. Enter your gateway URL: `https://prodaus.gateway.airia.ai`
5. Enter your gateway API key in the **Token** field

![Connect to Gateway](/images/excel_connect_gateway.png)

6. Click **Continue** — this will test the connection and open the add-in

You can now use the Claude add-in as normal.

![Claude Add-In](/images/excel_use_claude.png)
