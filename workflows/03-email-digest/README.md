# 03 - Email Digest

Daily email digest. Reads all undigested classified emails from the `email_triage` table, generates an AI summary grouped by category, sends it to Telegram, then marks emails as digested.

## Purpose

Provide a single daily Telegram message that recaps all emails processed by workflow `02-email-triage`, grouped by category, so nothing gets missed.

## Trigger

`n8n-nodes-base.scheduleTrigger` — runs once per day at 12:00.

## Flow

```
Schedule Trigger (12:00 daily)
  --> Postgres (query undigested emails from last 24 h)
  --> AI Agent (summarise by category, HTML format)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Telegram (send digest)
  --> Postgres (mark digest_sent = true)
```

## Database

Reads from table `email_triage` where `digest_sent = false` and `processed_at > NOW() - INTERVAL '24 hours'`.
Updates `digest_sent = true` after successful send.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration Notes

- Depends on `02-email-triage` to populate `email_triage` table
- Output format: HTML (Telegram parse mode)
- Error handling: routed to `00-error-workflow`
