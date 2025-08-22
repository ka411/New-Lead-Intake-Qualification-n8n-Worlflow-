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
