# Security Policy

## Public Repository Scope

This repository contains a public-safe demo and handoff package. It must not contain real API keys, access tokens, session cookies, production credentials, real customer data, or private infrastructure details.

## Secrets Handling

Use n8n Credentials, environment variables, or a secrets manager for runtime secrets.

Do not commit:

- API keys
- Telegram bot tokens
- YooKassa shop secrets
- Bpium session cookies
- Real customer emails, phone numbers, or payment links
- Production webhook URLs

## Supported Use

This project is intended as a demo/template for handoff documentation and secure automation packaging. For real client delivery, use a private repository and grant access individually.

## Reporting Issues

If you notice a leaked secret or sensitive data in this repository, revoke the affected secret first, then remove it from Git history and rotate all related credentials.
