# Setting up an Airia Gateway

## 1. Set Up a New Airia Gateway Configuration

Navigate to **Gateway → AI Gateway** and click **Add Configuration** to open the Create New Configuration page.

Give the gateway a name — for this example we'll use `Claude_Desktop` — then enable the **Anthropic AI Provider**.

Expanding the Anthropic AI Provider section reveals additional options:

- **Base URL** — override the default endpoint if needed
- **AI Service Authentication** — choose between Airia's Anthropic key or your own customer Anthropic key
- **Allowed Models** — restrict access to specific models (e.g. type `claude-sonnet-4-6` and press Tab to add it)

For this example, we'll leave all of these options at their defaults and proceed without any additional configuration.

Click **Create** in the bottom right corner to create the Gateway

## 2. Provision User Access to the Gateway

Now that the gateway is created, we need to provision access so that Airia users with end user permissions can use it.

On the Gateway Configurations page, locate the new gateway (e.g. `Claude_Desktop`) and click **Manage**.

Select the **User Access** tab. From here you can grant access to individual users or groups — for this example we'll add individual users.

Click **Add Users**, select the users you want to grant access to, then click **Add Selected**.

With users provisioned, the final step is to generate an API key for each one:

1. Click the **⋯** menu next to the user
2. Select **Manage API Keys**
3. Click **Create New Key**

Repeat this for each user you wish to provision an API key to.

## 3. Provision Gateway Budgets

Budgets can be set at two levels — the **gateway level** and the **per user level**. Both support daily, weekly, and monthly limits. User budgets contribute towards the overall gateway budget.

### Gateway Budget

Locate the gateway (e.g. `Claude_Desktop`) and click **Manage**, then select the **Budget** tab.

From here you can set a **Daily**, **Weekly**, and **Monthly** budget limit (e.g. `$100`). You can also configure notifications at specific spending thresholds (e.g. 80% of budget).

### User Budgets

To configure budgets at the individual user level, select **Configure Individual User Budget** and choose the users you want to apply a budget to. Only users provisioned in the previous step will appear in this list.

Select the users and click **Add Users**. For each user you can then set the same daily, weekly, and monthly limits at the per-user level.

### Monitoring Spend

As gateway traffic is recorded, spend will appear in **Budget History**, providing visibility into usage and costs across the gateway.

Once you have configured your budgets, click **Save** in the bottom right to apply.

## 4. Monitor Gateway Traffic

Navigate to **Gateway → Analytics → Gateway Monitoring** to view all traffic across your gateways.

From here you can filter logs by **date**, **model provider**, **user**, and **agent** to narrow down activity.

Click on any log entry to view the full call details, including:

- **Timeline** — end-to-end breakdown of the request
- **Request & Response** — full input and output content
- **Costs** — token usage and associated cost
- **Headers** — request headers for debugging

Individual logs can also be downloaded as JSON. To export multiple logs at once, use the **Export** button in the top right.