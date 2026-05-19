# Extended Details

## Examples

**Example 1 — Next.js subscription with trial**
- Input: "Add Stripe subscriptions to a Next.js 15 app, 14-day trial, monthly/yearly toggle"
- Action: Create Checkout Session API route → webhook handler with signature verification → user table linked via metadata.userId → Customer Portal route → `.env.example` updated
- Output: `app/api/checkout/route.ts`, `app/api/webhook/stripe/route.ts`, `app/api/portal/route.ts`, README with `stripe listen` instructions

**Example 2 — Debug webhook not delivering**
- Input: "Subscriptions create in Stripe but our DB is not updating"
- Action: Check webhook endpoint logs in Stripe dashboard → verify signature secret matches env → test with `stripe trigger customer.subscription.created` → identify missing event in handler switch
- Output: Diagnosis (handler missing case for `customer.subscription.created`), fix patch, regression test using mocked webhook payload
