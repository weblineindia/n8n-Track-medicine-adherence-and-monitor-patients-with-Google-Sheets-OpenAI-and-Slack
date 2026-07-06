# 🩺 AI-Powered Medicine Reminder & Patient Monitoring Workflow

*(n8n + Google Sheets + OpenAI + Slack)*

This workflow automates medicine reminders, tracks patient responses and alerts doctors in critical situations using n8n, Google Sheets, OpenAI and Slack.

### Quick Implementation Steps

- Connect your Google Sheets with patient data.
- Set up Slack (or replace it with Gmail or SMS nodes).
- Configure the Webhook node to receive patient replies.
- Add your OpenAI API key.
- Define reminder times in `HH:mm` format.
- Activate the workflow scheduler.

> 💡 You can replace Slack with Email (Gmail) or SMS APIs like Twilio.

## What It Does

This workflow automates the entire lifecycle of patient medicine adherence tracking. It periodically checks patient records and sends reminders when it's time to take medicine.

When a patient responds, the system captures the reply via a webhook and uses AI to classify the response into categories such as **taken**, **not taken**, **delayed**, or **unclear**. It then updates patient records and tracks missed doses.

If a patient repeatedly misses medication or shows signs of distress, the system flags it as critical and alerts the doctor immediately through a separate notification channel.

## Who's It For

- Healthcare providers and clinics
- Telemedicine platforms
- Caregivers managing patients
- Health-tech startups
- Remote patient monitoring systems

## Requirements

To use this workflow, you need:

- [**n8n account (Self-hosted or Cloud)**](https://n8n.partnerlinks.io/om1efg2qgvwi)
- Google Sheets account
- Slack account (or Email/SMS alternative)
- OpenAI API key
- Google Sheet with the following columns:
  - `Name`
  - `Phone`
  - `Medicine`
  - `Reminder Time`
  - `Last Response`
  - `Last Patient Message`
  - `Status`
  - `Missed_Count`

> ⚠️ **Reminder Time** must be in 24-hour format (`HH:mm`).
>
> Examples: `08:30`, `14:45`, `21:00`

## Webhook Input Format

Your system must send patient replies in the following format:

```json
{
  "phone": "string",
  "message": "string"
}
```

## How It Works

### 1. Scheduler Trigger

- Runs every minute.
- Checks all patient records.

### 2. Fetch Patient Records

- Reads all rows from Google Sheets.

### 3. Match Reminder Time

- Compares the current time with each patient's reminder time.

### 4. Send Reminder

- Sends a reminder via Slack (or Email/SMS alternative).

### 5. Receive Patient Response

- Webhook captures incoming patient responses.

### 6. AI Classification

Classifies responses into:

- **TAKEN**
- **NOT_TAKEN**
- **LATER**
- **CONFUSED**

Also detects critical conditions.

### 7. Parse Output

- Ensures the AI response is valid JSON.

### 8. Match Patient

- Matches the phone number with the corresponding Google Sheets record.

### 9. Update Missed Count

- **NOT_TAKEN** → +1
- **LATER** → +1
- **TAKEN** → Reset to 0

### 10. Check Critical Condition

Triggered when:

- `is_critical = true`

**OR**

- `Missed_Count >= 3` (configurable)

### 11. Critical Flow

- Update patient status to **CRITICAL**.
- Alert the doctor through a separate notification channel.

### 12. Normal Flow

- Update the patient record.
- Send an appropriate response:
  - **NOT_TAKEN** → Reminder
  - **LATER** → Gentle follow-up
  - **TAKEN** → Appreciation message

## How To Customize Nodes

### Replace Slack

You can replace Slack nodes with:

- Gmail (Email)
- Twilio (SMS)
- WhatsApp API

### Modify AI Logic

- Edit the OpenAI prompt.
- Add new classifications.
- Customize AI-generated responses.

### Change Reminder Logic

- Adjust scheduler frequency.
- Add timezone handling.

### Adjust Critical Threshold

- Change the `Missed_Count >= 3` condition to any value that fits your requirements.

## Add-ons (Enhancements)

- WhatsApp integration
- Dashboard for patient tracking
- Push notifications
- Database logging
- AI health insights
- Doctor reports

## Use Case Examples

- Chronic disease monitoring
- Elderly care
- Post-surgery tracking
- Clinical trials
- Remote patient monitoring

## Troubleshooting Guide

| Issue | Possible Cause | Solution |
|--------|----------------|----------|
| Reminder not sent | Time mismatch | Use `HH:mm` format |
| Patient not matched | Phone number mismatch | Ensure the same phone format is used |
| AI parsing error | Invalid JSON | Check the parsing node |
| Missed count issue | Wrong classification | Verify AI output |
| Webhook not working | Incorrect endpoint | Verify the webhook URL |
| Slack notifications not sent | Credential issue | Reconnect Slack |
| Critical alert not triggered | Condition issue | Adjust the critical threshold |

## Need Help?

If you need help customizing this workflow, integrating it with your healthcare systems, or extending it with AI-powered patient monitoring, electronic health record (EHR) integrations, advanced reporting, and multi-channel reminder automation, our **WeblineIndia** team is ready to assist.

Explore our **[Process Automation Solutions](https://www.weblineindia.com/process-automation-solutions.html)** or connect with our **[n8n workflow development experts](https://www.weblineindia.com/n8n-automation/)** to build, customize, and scale your business automation with confidence.
