# 01a - Tech Digest

Daily AI news digest aggregating RSS feeds from the major AI research labs and publications, summarised by an LLM and delivered to a Telegram channel.

## Purpose

Automatically collect, deduplicate, and summarise the day's top AI/tech headlines every morning. Delivers a formatted HTML digest to the tech Telegram channel.

## Trigger

`n8n-nodes-base.scheduleTrigger` — runs once per day (configured in node parameters).

## Flow

```
Schedule Trigger
  --> [RSS Feeds: OpenAI, HuggingFace, DeepMind, Anthropic, ...]
  --> Merge Items
  --> Filter (last 24 h)
  --> Code (format text)
  --> AI Agent (summarise)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Telegram (send digest)
```

## Sources

RSS feeds from: OpenAI Blog, HuggingFace, Google DeepMind, Anthropic, MIT Technology Review, The Verge AI, Wired AI, VentureBeat AI.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Groq API (native) | `YOUR_GROQ_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |
| Telegram Bot (tech channel) | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |
| Telegram Debug Bot | `YOUR_TELEGRAM_DEBUG_BOT_CREDENTIAL_ID` |

## Configuration Notes

- LLM model: `llama-3.3-70b-versatile` via Groq
- Output format: HTML (Telegram parse mode)
- Error handling: routed to `00-error-workflow`
