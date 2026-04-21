# 08 - AI Error Agent

Intelligent error analysis agent. Triggered via Telegram command or automatically on severe errors, it uses an LLM to diagnose workflow failures, suggest fixes, and send an actionable report.

## Purpose

Go beyond plain error notifications — use AI to interpret error context, identify likely root causes, and propose concrete remediation steps.

## Trigger

`n8n-nodes-base.telegramTrigger` — responds to Telegram commands (e.g. `/diagnose`), and/or chained from `00-error-workflow` for automatic analysis.

## Flow

```
Telegram Trigger (/diagnose) or Error context input
  --> Code (extract error context, workflow name, node name)
  --> AI Agent (analyse error, propose fix)
      |- Groq Chat Model (llama-3.3-70b-versatile)
  --> Telegram (send diagnostic report)
```

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Telegram Bot | `YOUR_TELEGRAM_CREDENTIAL_ID` |
| Groq via OpenAI-compat | `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` |

## Configuration Notes

- LLM model: `llama-3.3-70b-versatile` via Groq
- Designed to complement `00-error-workflow` with deeper analysis
- Error handling: routed to `00-error-workflow`
