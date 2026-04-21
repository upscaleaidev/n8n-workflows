# 06 - Prospection Veille

Automated competitive and market intelligence (veille) pipeline. Aggregates signals from web sources, filters for relevant prospects or topics, and sends a Telegram briefing.

## Purpose

Stay ahead of market movements and potential prospects by automatically monitoring web signals and summarising relevant findings.

## Trigger

`n8n-nodes-base.scheduleTrigger` — runs on a configurable schedule.

## Flow

```
Schedule Trigger
  --> HTTP Request (fetch signals / search results)
  --> Filter (relevance filter)
  --> AI Agent (analyse and summarise)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Postgres (persist findings)
  --> Telegram (send briefing)
```

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration Notes

- LLM model: `llama-3.3-70b-versatile` via Groq
- Output format: HTML (Telegram parse mode)
- Error handling: routed to `00-error-workflow`
