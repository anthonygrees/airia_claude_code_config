# Setting Up an MCP Gateway in Airia

## 1. Create the MCP Gateway

1. In Airia, navigate to **Gateway → MCP Gateway → Gateway List**

![MCP Gateway List](/images/mcp_gateway_list.png)

2. Click **Create Gateway** in the top-right corner
3. Give the MCP Gateway a name (e.g. `devtools`)
4. Select the apps you want to include — you can search by name or filter by **Featured** or by category
5. Click an app to add it to **Selected Apps**

![Selected Apps](/images/selected_apps.png)

---

## 2. Configure Tools

1. Click **Configure Tools** in the bottom-right corner
2. For each app, click the app name and authenticate with your credentials to see the available tools
3. Click **Available Tools** and select from the list
4. Repeat the credential and tool selection for each app added to the gateway

![Configure Tools](/images/configure_tools.png)

---

## 3. Add Gateway Instructions *(Optional)*

Gateway Instructions provide guidance to the LLM on how to use this MCP gateway. The instructions are surfaced when an agent calls a tool — useful for providing context such as Jira project keys, Slack channel names and IDs, or other integration-specific details.

![Gateway Instructions](/images/gateway_instructions.png)

---

## 4. Set Visibility and Save

1. In the bottom-left corner, set the gateway visibility:
   - **Personal** — only you can see this gateway
   - **Tenant-wide** — all users on your tenant can see it

   > **Note:** Visibility cannot be changed after the gateway is created.

2. Click **Save** to create the MCP Gateway

---

## 5. Monitor MCP Traffic

Navigate to **Gateway → Analytics → MCP Monitoring** to view traffic through your MCP Gateways.

From here you can see:
- Total requests and sessions
- Average request duration
- Traffic over time
- Top functions called
- A log of individual requests

![MCP Monitoring](/images/mcp_monitoring.png)

Click on any individual request to view an overview along with the full request and response details.

![MCP Request Details](/images/mcp_request_details.png)
