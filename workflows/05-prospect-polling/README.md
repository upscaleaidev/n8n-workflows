# 05 - Prospect Polling

Telegram-bot command handler for prospect management. Listens for `/prospects` commands and returns a formatted list of hot prospects stored in PostgreSQL.

## Purpose

Allow quick on-demand review of the sales pipeline directly from Telegram without opening a CRM or database client.

## Trigger

`n8n-nodes-base.telegramTrigger` — listens for incoming Telegram messages (webhook or long-poll).

## Flow

```
Telegram Trigger (/prospects command)
  --> Filter (check command is /prospects)
  --> Postgres (query hot prospects)
  --> Code (format response)
  --> Telegram (reply with prospect list)
```

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Telegram Bot | `YOUR_TELEGRAM_CREDENTIAL_ID` |
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |

## Configuration Notes

- Only responds to authorised chat ID (`YOUR_TELEGRAM_CHAT_ID`)
- Reads from the same `email_triage` table used by workflows `02` and `03`
- Error handling: routed to `00-error-workflow`
