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

High budgets: “Please book a call at this lin.k”

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
