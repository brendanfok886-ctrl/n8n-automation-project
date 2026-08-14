# Meta Inbox Customer Service Automation

An AI-powered customer service workflow built with **n8n** for handling Meta inbox messages.

The workflow receives incoming customer messages, validates them, checks for duplicates, retrieves approved help-center content, classifies each request with AI, and then either:

* Sends an automatic reply for supported general questions, or
* Escalates the request to a human support team through ClickUp.

---

## Features

* Facebook Messenger message handling
* Instagram routing structure
* Meta webhook integration
* Message validation
* Duplicate message protection
* Approved CMS knowledge retrieval
* AI-powered intent classification
* Source-grounded customer replies
* Automatic reply routing
* Human escalation routing
* ClickUp task creation
* Customer acknowledgement messages
* Interaction logging
* Workflow error handling
* Test-mode safety controls

---

## Built With

* **n8n** — workflow automation
* **OpenAI** — intent classification and response generation
* **Meta Graph API** — Messenger and Instagram messaging
* **ClickUp** — human support escalation
* **CMS API** — approved customer-service knowledge
* **n8n Data Tables** — message logging and duplicate prevention

---

## How It Works

```text
Customer Message
        ↓
Meta Webhook
        ↓
Validate Event
        ↓
Ignore Invalid / Unsupported Events
        ↓
Check Duplicate Message ID
        ↓
Normalize Message and Channel
        ↓
Fetch Approved Help Content
        ↓
Retrieve Relevant Sources
        ↓
AI Classification
        ↓
Validate AI Output
        ↓
   ┌────┴────┐
   ↓         ↓
AUTO_REPLY   ESCALATE
   ↓         ↓
Send Reply   Create ClickUp Task
   ↓         ↓
Log          Send Acknowledgement
             ↓
             Log
```

---

## AI Safety Logic

The AI is designed to answer only when the response is fully supported by approved content retrieved from the configured knowledge source.

Automatic replies are intended for general informational topics such as:

* How to buy
* How to sell
* Authentication overview
* Sizing and resizing guidance
* General shipping information

The workflow requires:

* High intent confidence
* At least one approved source
* Full source support
* An allowed topic
* No sensitive or transactional information

If these conditions are not met, the request is escalated.

---

## Requests That Are Escalated

Examples include:

* Refunds
* Returns requiring review
* Order status
* Tracking
* Missing or delayed orders
* Payment issues
* Seller payout questions
* Chargebacks
* Disputes
* Account access issues
* Fraud allegations
* Authentication disputes
* Complaints
* Cancellation requests
* Legal matters
* Privacy requests
* Questions requiring live account or transaction data

When the workflow is uncertain, it escalates instead of generating an unsupported answer.

---

## Human Escalation

When a request requires human support, the workflow can:

1. Classify the request as `ESCALATE`
2. Create a ClickUp task
3. Include the original customer message
4. Include detected intent and topic
5. Include AI confidence
6. Include the escalation reason
7. Include an AI-generated summary
8. Include recommended staff action
9. Send a neutral acknowledgement
10. Log the interaction

---

## Message Logging

The workflow uses an n8n Data Table for message logging and duplicate prevention.

Typical stored fields include:

```text
message_id
conversation_id
customer_id
channel
event_timestamp
original_message
detected_intent
topic
confidence
decision
source_titles
source_urls
customer_reply
escalation_reason
clickup_task_id
execution_id
reply_delivery_status
```

The `message_id` is used to prevent the same message from being processed multiple times.

---

## Requirements

Before using this workflow, you need:

* An n8n instance
* A Meta Developer App
* A Facebook Page
* Meta / Facebook Graph API credentials
* An Instagram Professional account if Instagram will be enabled
* An OpenAI API account
* A ClickUp account
* A ClickUp List for escalated support tasks
* An n8n Data Table for message logging
* Access to an approved CMS / help-center API

---

## Installation

### 1. Download the workflow

Download:

```text
meta-inbox-customer-service-public-template.json
```

### 2. Import into n8n

In n8n:

```text
Workflows → Import from File
```

Select the downloaded JSON file.

The public template is exported as **inactive** for safety.

---

## Configuration

The public version contains placeholders instead of private deployment information.

Configure values such as:

```text
CONFIGURE_META_VERIFY_TOKEN
CONFIGURE_MESSENGER_TEST_ID
CONFIGURE_INSTAGRAM_TEST_ID
CONFIGURE_CLICKUP_LIST_ID
CONFIGURE_MESSAGE_LOG_DATA_TABLE_ID
CONFIGURE_WEBHOOK_PATH
CONFIGURE_CMS_API_HOST
```

---

## Configure Credentials

After importing the workflow, reconnect your own credentials inside n8n.

### OpenAI

Connect your OpenAI credential to the AI model node.

### Meta / Facebook

Connect the Meta / Facebook Graph API credential used by the Messenger nodes.

### Instagram

Connect the required Instagram / Meta credential before enabling Instagram outbound messaging.

### ClickUp

Connect your ClickUp OAuth credential and configure the ClickUp List used for escalations.

---

## Configure the Meta Webhook

Replace:

```text
CONFIGURE_WEBHOOK_PATH
```

with your own webhook path.

Your webhook URL will look similar to:

```text
https://YOUR-N8N-DOMAIN/webhook/YOUR-WEBHOOK-PATH
```

Set a private verification token in both n8n and your Meta Developer App.

Do not commit the real verification token to GitHub.

---

## Configure the Data Table

Create or connect an n8n Data Table for message logging.

Replace:

```text
CONFIGURE_MESSAGE_LOG_DATA_TABLE_ID
```

with your own Data Table ID.

---

## Configure the CMS

Replace:

```text
CONFIGURE_CMS_API_HOST
```

with the approved CMS or help-center API host for your environment.

The AI should only use content retrieved from approved sources.

---

## Test Mode

The workflow includes test safety controls.

Before production use:

1. Connect your Meta credentials
2. Connect the correct Facebook Page
3. Connect the Instagram Professional account if needed
4. Configure the webhook
5. Set a private verification token
6. Add test customer IDs
7. Reconnect OpenAI
8. Reconnect ClickUp
9. Configure the ClickUp List
10. Configure the Data Table
11. Configure the CMS API
12. Test automatic replies
13. Test escalations
14. Test acknowledgements
15. Test logging
16. Complete user acceptance testing
17. Only then enable production sending

---

## Usage

### Automatic Reply

```text
Customer Message
→ Meta Webhook
→ Validate
→ Retrieve Approved Source
→ AI Classification
→ AUTO_REPLY
→ Send Customer Reply
→ Log Interaction
```

### Human Escalation

```text
Customer Message
→ Meta Webhook
→ AI Classification
→ ESCALATE
→ Create ClickUp Task
→ Send Customer Acknowledgement
→ Log Interaction
```

---

## Security

This repository contains an anonymized public template.

Do not commit real:

```text
OpenAI API keys
Meta access tokens
Facebook Page access tokens
Instagram access tokens
ClickUp access tokens
Webhook verification tokens
Passwords
Production customer IDs
Private internal URLs
Confidential company information
```

Store credentials securely inside n8n.

If a secret is accidentally committed to GitHub, remove it and rotate or revoke it immediately.

---

## Production Notice

This workflow should not be used with real customers immediately after import.

Before production deployment:

* Replace all placeholders
* Reconnect all credentials
* Verify Meta permissions
* Verify webhook configuration
* Verify ClickUp configuration
* Verify the n8n Data Table
* Verify CMS connectivity
* Test all auto-reply rules
* Test all escalation rules
* Test error handling
* Complete final platform testing

---

## Workflow File

```text
meta-inbox-customer-service-public-template.json
```

---

## Project Purpose

This project demonstrates how n8n can combine:

* Meta messaging
* AI customer-service classification
* Approved knowledge retrieval
* Automated customer replies
* Human escalation
* Support task creation
* Logging and monitoring

into a single customer-service automation workflow.
