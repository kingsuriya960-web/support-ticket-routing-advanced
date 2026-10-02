# Support Ticket Routing - Advanced

An automated n8n workflow that routes incoming support emails to the right team, sends instant confirmations, and tracks response SLAs.

## Features

- **AI-powered classification:** Reads incoming emails and categorizes by type (billing, technical, account, sales, general) and priority (urgent, high, normal, low).
- **Automatic routing:** Routes tickets to the right assignee based on a configurable routing table.
- **SLA tracking:** Each ticket gets a due time based on priority (urgent: 1h, high: 4h, normal: 24h, low: 72h).
- **Customer auto-reply:** Sends instant confirmation with ticket ID.
- **Urgent alerts:** Telegram notifications for urgent tickets and sales leads.
- **Overdue reminders:** Checks every 30 minutes for past-due tickets and alerts the assignee.
- **Automated inbox:** Logs all tickets in a Google Sheet, filters out automated senders.

## Setup

### 1. Create the Google Sheet

Create a new Google Sheet named `support ticket` with two tabs:

**Tab: Sheet1** (main ticket log)
| ticket_id | received_at | customer_email | customer_name | subject | summary | category | product | priority | assigned_team | assigned_to | status | gmail_message_id | sla_due_at | reminder_sent |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**Tab: routing** (team assignments)
| category | team_name | email |
|---|---|---|
| billing | Billing | your-email@gmail.com |
| technical | Technical Support | your-email@gmail.com |
| account | Account & Access | your-email@gmail.com |
| sales | Sales | your-email@gmail.com |
| general | General Support | your-email@gmail.com |

### 2. Import the Workflow

1. In n8n, go to **Workflows** → **Create workflow** → **⋯ menu** → **Import from file**.
2. Select `Support_Ticket_Routing_Advanced_Public.json`.
3. Click **Confirm**.

### 3. Connect Credentials

Open each of these nodes and connect your accounts:

- **New Support Email**: Gmail (support inbox account)
- **Classify Ticket**: OpenAI (API key)
- **Notify Assignee, Auto Reply Customer, Mark Email Handled, Send Overdue Reminder**: Gmail (same account)
- **Routing Directory, Create Ticket, Get Open Tickets, Mark Reminder Sent**: Google Sheets
- **Urgent Telegram Alert**: Telegram (optional; skip if you don't want alerts)

### 4. Configure Nodes

**Routing Directory:**
- Select your `support ticket` sheet and `routing` tab.

**Create Ticket:**
- Select your `support ticket` sheet and `Sheet1` tab.

**Get Open Tickets:**
- Select your `support ticket` sheet and `Sheet1` tab.

**Urgent Telegram Alert** (optional):
- Get your Telegram chat ID from **@userinfobot** in Telegram.
- Paste it into the **Chat ID** field.
- Ensure your bot is active and has sent you a message once.

### 5. Test

1. Send a test email to your support inbox from another account.
2. Click **Execute workflow** on the **New Support Email** branch.
3. Check that:
   - A new row appears in `Sheet1`.
   - An alert email arrives at the assignee.
   - An auto-reply reaches the customer.
   - (If urgent) A Telegram message is sent.

### 6. Activate

Toggle **Active** (top right) to make the workflow run every minute.

## Customization

**Change SLA times:** Edit the **Build Ticket** node, field `sla_hours`. Replace the numbers:
```
{urgent:1, high:4, normal:24, low:72}
```

**Change routing:** Edit the `routing` tab in your sheet. Add more rows for custom categories or change email addresses.

**Change assignees:** Update the email in any row of the `routing` tab.

**Add a third-party integration:** Replace the Telegram node with Slack, Microsoft Teams, or any other service n8n supports.

## How to Use Day-to-Day

1. **Support inbox:** Check incoming emails at your support inbox. The workflow processes them automatically.
2. **Alert emails:** You receive an alert for each new ticket with the full details.
3. **Customer replies:** Replies become new tickets (you can add reply matching later).
4. **Close tickets:** Edit the `status` column in `Sheet1` from `open` to `closed`. Closed tickets no longer trigger overdue reminders.
5. **Overdue check:** Every 30 minutes, the workflow checks for open tickets past their SLA and sends you a reminder.

## For Resale

- Duplicate this workflow for each client.
- Create a new Google Sheet for each client.
- Charge recurring: $99–299/month depending on features.
- Your clients manage their routing via the `routing` tab.

## Support

For issues or feature requests, open an issue on GitHub.

## License

MIT
