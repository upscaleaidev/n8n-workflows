# 02 - Email Triage

Automated email classification pipeline. Polls Gmail every 5 minutes, classifies each new email with an LLM into one of 8 categories, stores results in PostgreSQL, applies Gmail labels, and sends Telegram alerts for high-priority messages.

## Purpose

Eliminate manual inbox triaging. Every unread email is classified, persisted, labelled, and — when urgent — escalated to Telegram immediately.

## Trigger

`n8n-nodes-base.gmailTrigger` — polls inbox every 5 minutes for new unread messages.

## Flow

```
Gmail Trigger (poll every 5 min)
  --> Code (extract fields)
  --> AI Agent (classify email)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Postgres (upsert into email_triage)
  --> Switch (on category)
      |- URGENT          --> Telegram alert + Gmail label
      |- PROSPECT_CHAUD  --> Telegram alert + Gmail label
      |- [other 6]       --> Gmail label only
```

## Email Categories

| Category | Description |
|----------|-------------|
| URGENT | Requires same-day action |
| PROSPECT_CHAUD | Hot sales lead |
| CLIENT | Existing client message |
| FACTURATION | Invoice / payment |
| NEWSLETTER | Newsletter / digest |
| RECRUTEMENT | Recruitment / job offer |
| SPAM | Junk / unsolicited |
| AUTRE | Everything else |

## Database

Table: `email_triage`

Key columns: `message_id`, `subject`, `sender`, `category`, `summary`, `processed_at`, `digest_sent`, `gmail_label_applied`.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Gmail OAuth2 | `YOUR_GMAIL_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration Notes

- LLM model: `llama-3.3-70b-versatile` via Groq (OpenAI-compat endpoint)
- Telegram alerts only for `URGENT` and `PROSPECT_CHAUD`
- Deduplication via `message_id` UPSERT in Postgres
- Error handling: routed to `00-error-workflow`
