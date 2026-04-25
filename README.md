# n8n Workflows

Public export of my personal n8n automation stack. All credential IDs, chat IDs, instance URLs, user IDs, and email addresses have been sanitised. Import the `workflow.json` files into your own n8n instance and replace the placeholder values with your own credentials.

## Architecture Overview

```
                    ┌──────────────────────────────────┐
                    │  00 - Error Workflow             │
                    │  (global error handler)          │
                    └──────────────┬───────────────────┘
                                   │ errorWorkflow
         ┌─────────────────────────┼──────────────────────────────────┐
         │                         │                                  │
┌────────▼──────────┐   ┌─────────▼───────────┐   ┌──────────────────▼────┐
│  01a Tech Digest  │   │  02 Email Triage    │   │  07 Stripe Onboarding │
│  01b Crypto       │   │  03 Email Digest    │   └───────────────────────┘
│  01c Finance      │   │                     │
│  01d World        │   │  05 Prospect Poll   │   ┌───────────────────────┐
└───────────────────┘   │  05b Prospect Agent │   │  04 Self Monitoring   │
                        │  06 Veille          │   └───────────────────────┘
                        └─────────────────────┘
                                                  ┌───────────────────────┐
                                                  │  08 AI Error Agent    │
                                                  └───────────────────────┘
```

## Workflows

| Folder | Name | Description |
|--------|------|-------------|
| [`00-error-workflow`](workflows/00-error-workflow/) | Error Workflow | Global error handler — sends Telegram alert on any workflow failure |
| [`01a-tech-digest`](workflows/01a-tech-digest/) | Tech Digest | Daily AI/tech news from OpenAI, HuggingFace, DeepMind, Anthropic, etc. |
| [`01b-crypto-digest`](workflows/01b-crypto-digest/) | Crypto Digest | Daily crypto news from CoinDesk, CoinTelegraph, Decrypt, etc. |
| [`01c-finance-digest`](workflows/01c-finance-digest/) | Finance Digest | Daily finance/macro news from Bloomberg, FT, Reuters, etc. |
| [`01d-world-digest`](workflows/01d-world-digest/) | World Digest | Daily world news from BBC, Reuters, NYT, Al Jazeera, etc. |
| [`02-email-triage`](workflows/02-email-triage/) | Email Triage | AI email classification (8 categories) with Gmail labelling + Telegram alerts |
| [`03-email-digest`](workflows/03-email-digest/) | Email Digest | Daily digest of classified emails sent to Telegram |
| [`04-self-monitoring`](workflows/04-self-monitoring/) | Self Monitoring | n8n instance health report delivered to Telegram |
| [`05-prospect-polling`](workflows/05-prospect-polling/) | Prospect Polling | On-demand Telegram command to list hot prospects from database |
| [`05b-prospect-handler`](workflows/05b-prospect-handler/) | Prospect Handler | AI-assisted Telegram conversation for prospect follow-up |
| [`06-prospection-veille`](workflows/06-prospection-veille/) | Prospection Veille | Automated market/competitive intelligence briefing |
| [`07-stripe-onboarding`](workflows/07-stripe-onboarding/) | Stripe Onboarding | Full post-payment onboarding: parse Stripe webhook, save client, send welcome email |
| [`08-ai-error-agent`](workflows/08-ai-error-agent/) | AI Error Agent | AI-powered error diagnosis and fix suggestions via Telegram |

## Tech Stack

- **Automation**: [n8n](https://n8n.io) (self-hosted)
- **LLM**: [Groq](https://groq.com) — `llama-3.3-70b-versatile`
- **Messaging**: Telegram Bot API
- **Email**: Gmail OAuth2
- **Database**: PostgreSQL
- **Payments**: Stripe Webhooks

## Credentials Setup

Before importing any workflow, create the following credentials in your n8n instance:

| Placeholder | Credential Type | Notes |
|------------|----------------|-------|
| `YOUR_POSTGRES_CREDENTIAL_ID` | Postgres | Your PostgreSQL connection |
| `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` | Telegram Bot API | Main notification bot |
| `YOUR_TELEGRAM_DEBUG_BOT_CREDENTIAL_ID` | Telegram Bot API | Debug/test bot |
| `YOUR_TELEGRAM_CREDENTIAL_ID` | Telegram Bot API | Bot used for interactive commands |
| `YOUR_GROQ_CREDENTIAL_ID` | Groq API | Native Groq credential |
| `YOUR_GROQ_OPENAI_COMPAT_CREDENTIAL_ID` | OpenAI API | Groq via OpenAI-compatible endpoint |
| `YOUR_GMAIL_CREDENTIAL_ID` | Gmail OAuth2 | Gmail send + read access |
| `YOUR_N8N_API_CREDENTIAL_ID` | n8n API | For self-monitoring workflow |

Additionally, update the following values in workflow node parameters:

| Placeholder | Description |
|------------|-------------|
| `YOUR_TELEGRAM_CHAT_ID` | Your personal or group Telegram chat ID |
| `https://your-n8n-instance.com` | Your n8n instance base URL |
| `YOUR_N8N_PROJECT_ID` | Your n8n project ID |
| `YOUR_N8N_USER_ID` | Your n8n user ID |
| `YOUR_EMAIL` | Your email address |

## Importing Workflows

1. Open your n8n instance
2. Go to **Workflows** > **Import from file**
3. Select the `workflow.json` from the desired folder
4. Update all credential references to point to your own credentials
5. Enable the workflow

## Error Handling

All workflows reference `00-error-workflow` in their `settings.errorWorkflow` field. Import and activate `00-error-workflow` first, then update the `errorWorkflow` ID in each other workflow's settings to match the ID assigned by your n8n instance.

## License

MIT — feel free to use, adapt, and share these workflows.
