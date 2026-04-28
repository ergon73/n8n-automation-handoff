# n8n Automation Handoff

Public handoff package for a secure n8n CRM orders/payments workflow.

The workflow demonstrates how to package an automation for transfer to another specialist or client: sanitized workflow export, architecture description, integration notes, user guide, maintenance guide, support regulation, and change log.

## What The Workflow Does

The demo workflow processes a client request end-to-end:

1. Receives a form submission in n8n.
2. Minimizes the submitted data before sending it to AI.
3. Uses an AI model through ProxyAPI/OpenAI to estimate the project budget and draft a short offer.
4. Creates an order in Bpium CRM.
5. Creates a YooKassa payment link.
6. Writes the payment data back to Bpium.
7. Sends a safe Telegram notification without exposing client contact details.
8. Checks that the CRM order is available through the API.

## Repository Structure

```text
.
├── handoff-package/
│   ├── 00_README_пакет_передачи_NPr11.md
│   ├── 01_workflow_export_TEST_NPr10_CRM_Orders_Payments_Safe_sanitized.json
│   ├── 02_workflow_screenshot.md
│   ├── 02_architecture_workflow_screenshot.png
│   ├── 03_passport_automation.md
│   ├── 04_architecture_description.md
│   ├── 05_integrations_dependencies.md
│   ├── 06_user_instruction.md
│   ├── 07_maintenance_instruction.md
│   ├── 08_support_regulation.md
│   ├── 09_change_log.md
│   └── 10_submission_text.md
├── env.example
├── SECURITY.md
├── LICENSE
└── README.md
```

## Public Safety Notes

This repository is intentionally public-safe:

- Real API keys, tokens, cookies, and credentials are not included.
- Demo form data is anonymized.
- The workflow uses placeholder credentials and environment variables.
- The Bpium workspace URL is parameterized through `BPIUM_BASE_URL_TEST`.
- In real client work this package should be delivered through a private repository with individual access.

## Required Runtime Configuration

Before importing and running the workflow in n8n, configure credentials and environment variables from `env.example`.

Expected credentials:

- `ProxyAPI Header Auth` for ProxyAPI/OpenAI requests.
- `YooKassa Basic Auth` for payment creation.
- `Telegram Bot API` for Telegram notifications.
- Bpium API access via `BPIUM_SESSION_COOKIE_TEST` or a more secure credential/vault approach.

## Import Notes

Import the workflow JSON from:

```text
handoff-package/01_workflow_export_TEST_NPr10_CRM_Orders_Payments_Safe_sanitized.json
```

After import:

1. Assign local n8n credentials to AI, YooKassa, and Telegram nodes.
2. Configure required environment variables.
3. Run the workflow in test mode.
4. Check the `Executions` tab and verify that CRM, payment, Telegram, and final check nodes complete successfully.
