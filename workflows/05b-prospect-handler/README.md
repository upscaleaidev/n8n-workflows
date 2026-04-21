# 05b - Prospect Handler

AI-powered Telegram conversation assistant for prospect follow-up. Handles incoming Telegram messages from prospects, uses an LLM to draft contextual replies, and persists interaction logs to PostgreSQL.

## Purpose

Assist with sales follow-up by drafting AI-generated responses to prospect messages and maintaining a conversation history in the database.

## Trigger

`n8n-nodes-base.telegramTrigger` — listens for incoming Telegram messages.

## Flow

```
Telegram Trigger (incoming message)
  --> Code (extract sender, message, context)
  --> Postgres (load conversation history)
  --> AI Agent (draft reply)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Postgres (save interaction log)
  --> Telegram (send drafted reply)
```

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Telegram Bot | `YOUR_TELEGRAM_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |

## Configuration Notes

- Companion to `05-prospect-polling`
- LLM model: `llama-3.3-70b-versatile` via Groq
- Error handling: routed to `00-error-workflow`
