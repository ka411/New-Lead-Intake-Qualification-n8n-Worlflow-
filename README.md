# New-Lead-Intake-Qualification-n8n-Worlflow-

An n8n workflow that collects new leads via a web form, saves them to Google Sheets, qualifies by budget, sends tailored auto-reply emails, and notifies the team by email.

✨ Features

Form trigger with fields: First Name, Last Name, Email, Budget (dropdown), Message

Google Sheets append/update (de-duplicates by Email)

Lead qualification

Filters out very low budgets

Branches on budget ranges for tailored responses

Auto-responses via Gmail

Mid-tier budgets: “We’ll call you this week.”

High budgets: “Please book a call at this link.”

Internal alert to the team with full lead details


🧩 Node-by-node (High Level)

On form submission — starts the flow when a “Test Form” is submitted.

Google Sheets (appendOrUpdate) — writes lead data with columns:

First Name, Last Name, Email, Message, Budget, Date, Rejected

Rejected is auto-computed: FALSE for 1,000+, TRUE otherwise.

Filter — only continue when Budget ≠ 0–100.

Switch — route by budget:

100–1,000 → Gmail message A (callback later this week)

1,000+ → Gmail message B (booking link)

Merge — join both branches.

Gmail — send an internal “New Lead” email with the captured details.


🗂️ Google Sheets Schema

Matching column: Email (prevents duplicates when the same email submits again)

Suggested headers in your sheet (row 1):

First Name | Last Name | Email | Budget  | Message | Date | Rejected

The workflow maps form fields directly to these columns and stamps Date with the current day.


📬 Email Templates (Gmail)

Budget 100–1,000
Subject: Hey, thanks for inquiring with ABC company
Body: Acknowledge inquiry → say you’ll call later this week.

Budget 1,000+
Subject: Hey, thanks for inquiring with ABC company
Body: Acknowledge → ask them to book a call at your calendar link.

Internal notification
Subject: New Lead
Body: Name, Email, Budget, Message (sent to the configured internal address).

Tip: personalize subjects/bodies and replace the placeholder booking link with your real calendar URL.


🛠️ Setup

Import the JSON into n8n: Settings → Workflows → Import from File.

Credentials

Google Sheets OAuth2: connect the Google account that owns your spreadsheet.

Gmail OAuth2: the account that will send emails (and the internal alert).

Google Sheet

Create a sheet with the headers above (exact names recommended).

Paste its Spreadsheet ID into the Google Sheets node (already set in the JSON—replace with your own if needed).

Form trigger

The Form Trigger node defines the fields and handles submissions.

Host it behind your site or share the form endpoint depending on your n8n deployment.

Booking link & recipient

Update the booking URL in the high-budget Gmail node.

Update the internal notification recipient email.

🧪 Test Checklist

Submit a test lead with Budget 0–100 → should be filtered out (no emails sent beyond Sheets write).

Submit 100–1,000 → receive callback-style auto-reply; internal notification arrives.

Submit 1,000+ → receive booking-link auto-reply; internal notification arrives.

Re-submit with the same Email → row should update rather than duplicate.

🔒 Security & Privacy

Do not commit API credentials. Use n8n’s credential store or environment variables.

If handling personal data (emails, messages), comply with GDPR (data retention, export/delete on request).

Consider rate-limiting or CAPTCHA on your public form endpoint.

🧭 Customization Ideas

Add a Slack/Discord notification node for the sales channel.

Replace the “Rejected” heuristic with a scoring node (e.g., budget + message keywords).

Push qualified leads to a CRM (HubSpot, Pipedrive, etc.).

Localize autoresponders by form language.

🐳 Deployment

Self-host n8n via Docker or use n8n Cloud.

Ensure public reachability for the form webhook (reverse proxy or n8n Tunnel).

Back up your workflow JSON and sheet regularly.
