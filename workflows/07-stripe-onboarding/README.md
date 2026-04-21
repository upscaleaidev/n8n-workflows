# 07 - Stripe Onboarding

Automated post-payment onboarding pipeline. Receives Stripe `checkout.session.completed` webhook events, identifies the purchased product, saves the client to PostgreSQL, sends a personalised welcome email via Gmail, and notifies via Telegram.

## Purpose

Fully automate the client onboarding sequence after a Stripe payment — zero manual steps from payment to welcome email delivery.

## Trigger

`n8n-nodes-base.webhook` — listens on path `/stripe-payment` for POST events from Stripe.

## Flow

```
Stripe Webhook (POST /stripe-payment)
  --> Code Parse Stripe   (extract email, name, amount, session_id)
  --> Code Map Product    (map amount_cents → product name + next_action)
  --> Postgres Save Client (upsert into clients table)
  --> Code Email Body     (build personalised subject + body by next_action)
  --> Gmail Welcome       (send welcome email to client)
  --> Telegram New Client (internal alert: email + product name)
```

## Product Map

| Price (cents) | Product | Next Action |
|--------------|---------|-------------|
| 9 700 | Templates Pack | send_templates |
| 14 700 | Automation Audit | send_calendly |
| 19 700 | Monthly Maintenance | send_onboarding |
| 29 700 | n8n Autonomy Training | send_training |
| 49 700 | Starter Pack | send_onboarding |
| 79 700 | Growth Retainer | send_onboarding_priority |
| 149 700 | Scale Pack | send_onboarding |

## Database

Table: `clients`

Key columns: `email`, `name`, `product_name`, `amount_euros`, `stripe_session_id`, `next_action`.
Deduplication via `ON CONFLICT (stripe_session_id) DO NOTHING`.

## Credentials Required

| Credential | Placeholder |
|-----------|-------------|
| PostgreSQL | `YOUR_POSTGRES_CREDENTIAL_ID` |
| Gmail OAuth2 | `YOUR_GMAIL_CREDENTIAL_ID` |
| Telegram Bot | `YOUR_TELEGRAM_BOT_CREDENTIAL_ID` |

## Configuration Notes

- Webhook path must be registered as the Stripe webhook endpoint in the Stripe dashboard
- Error handling: routed to `00-error-workflow`
