# 04 - Self Monitoring

Instance health monitor. Periodically calls the n8n API to check workflow execution statistics and sends a Telegram report with counts of successful and failed executions.

## Purpose

Keep track of automation health without logging into the n8n UI. Surfaces failures early and provides a daily summary of execution throughput.

## Trigger

`n8n-nodes-base.scheduleTrigger` — runs on a configurable schedule (e.g. daily).

## Flow

```
Schedule Trigger
  --> n8n API (list recent executions)
  --> Code (aggregate stats: success/error counts per workflow)
  --> Telegram (send health report)
```

## Report Format

Summary of execution counts grouped by workflow, highlighting any errors from the past 24 hours.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| n8n API | `YOUR_N8N_API_CREDENTIAL_ID` |
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration Notes

- Uses the n8n public API (`/executions` endpoint)
- Error handling: routed to `00-error-workflow`
