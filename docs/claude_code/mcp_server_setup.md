# Connecting an MCP Gateway to Claude Code CLI

---

## 1. Authenticate Apps in the MCP Gateway

1. In Airia, navigate to **Gateway → MCP Gateway → Gateway List**
2. Open the MCP Gateway you want to connect to Claude Code CLI
3. For each app in the gateway, click the app icon and authenticate with your credentials

   > **Note:** This must be done for each app in the gateway. Each icon should show a green indicator when authenticated.

![Authenticate Apps](/images/authenticate_apps.png)

---

## 2. Get the Claude Code CLI Command

1. On the MCP Gateway page, click **Client Setup → Claude Code**
2. Copy the command beginning with `claude mcp add ...`

![Client Setup - Claude Code](/images/setup_claude_code.png)

---

## 3. Add the MCP Server

**To add to a specific project folder:**

1. Open your terminal
2. Navigate to the project folder you want to add the MCP gateway to
3. Run the following command:

```bash
claude mcp add --transport http <gateway-name> "https://prodaus.mcp-gateway.airia.ai/gateway/<your-gateway-id>/mcp"
```

**To add at the user level (available across all projects):**

```bash
claude mcp add --scope user --transport http <gateway-name> "https://prodaus.mcp-gateway.airia.ai/gateway/<your-gateway-id>/mcp"
```

---

## 4. Authenticate in Claude Code CLI

1. In your terminal, open Claude Code with the command `claude`
2. Type `/mcp` to open the MCP server management view
3. Select the MCP gateway and press **Enter**
4. Select **Authenticate** and press **Enter**

![Authenticate MCP](/images/authenticate_mcp.png)

5. You will be prompted to sign in to Airia
6. Once complete, type `/mcp` again. The gateway should now show **Authenticated**

![Connected MCP](/images/connected_mcp.png)

7. Press **Escape** to return to the Claude Code window

You can now call tools through the MCP Gateway directly from Claude Code CLI.

---

## 5. Calling Tools Through the MCP Gateway

Once connected, you can interact with your integrated apps using natural language. No need to manually call APIs or write queries. Claude will determine the appropriate tool to use and make the call through Airia's MCP Gateway automatically.

For example, if your MCP Gateway includes tools from the Atlassian MCP, you can simply ask:

> *"Give me all the Jira tickets I've raised in the last month"*

Claude will use the available tools to query the relevant data and return the results directly in the conversation.

---

## 6. Remove an MCP Server

To remove an MCP server from Claude Code CLI, run the following command:

```bash
claude mcp remove <gateway-name>
```
