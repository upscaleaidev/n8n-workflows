# 00 - Error Workflow

Global error handler for all n8n workflows. When any workflow fails, n8n automatically routes the error here.

## Purpose

Catch unhandled errors across the entire n8n instance and send an immediate Telegram alert so failures are never missed.

## Trigger

`n8n-nodes-base.errorTrigger` — fires automatically whenever another workflow throws an uncaught exception.

## Flow

```
Error Trigger --> Send Telegram Alert
```

## Nodes

| Name | Type | Description |
|------|------|-------------|
| Error Trigger | errorTrigger | Receives error context from any failing workflow |
| Send a text message | telegram | Sends alert with workflow name, error message and timestamp |

## Telegram Message Format

```
⚠️ ERREUR WORKFLOW
Workflow : <workflow name>
Erreur   : <error message>
Heure    : <HH:mm>
```

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration

Set this workflow's ID as the `errorWorkflow` in the settings of every other workflow so errors are routed here automatically.
