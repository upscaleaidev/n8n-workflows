# 01b - Crypto Digest

Daily cryptocurrency news digest aggregating RSS feeds from the major crypto publications, summarised by an LLM and delivered to a Telegram channel.

## Purpose

Automatically collect, filter, and summarise the day's top crypto headlines. Delivers a formatted HTML digest to the crypto Telegram channel.

## Trigger

`n8n-nodes-base.scheduleTrigger` — runs once per day.

## Flow

```
Schedule Trigger
  --> [RSS Feeds: CoinDesk, CoinTelegraph, Decrypt, ...]
  --> Merge Items
  --> Filter (last 24 h)
  --> Code (format text)
  --> AI Agent (summarise)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Telegram (send digest)
```

## Sources

RSS feeds from: CoinDesk, CoinTelegraph, Decrypt, The Block, Bitcoin Magazine.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Groq API (native) | `YOUR_GROQ_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| Telegram Bot (crypto channel) | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |
| Telegram Debug Bot | `YOUR_TELEGRAM_DEBUG_BOT_CREDENTIAL_ID` |

## Configuration Notes

- LLM model: `llama-3.3-70b-versatile` via Groq
- Output format: HTML (Telegram parse mode)
- Error handling: routed to `00-error-workflow`
