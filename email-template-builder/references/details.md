# Extended Details

## Examples

### Example 1 — New product needs full transactional set
- Input: "Set up Resend with welcome, verify, password-reset, invoice"
- Action: Scaffold `emails/` with shared layout + button, 4 templates, Resend client wrapper, preview server, plain-text fallbacks, dark-mode media queries, locale shells
- Output: Production-ready `emails/` directory + send service + preview running on localhost

### Example 2 — Migrating provider from SendGrid to Postmark
- Input: "Switch from SendGrid to Postmark, keep all templates"
- Action: Add Postmark adapter to unified send interface, update env config, run preview pass, send test to internal seed list, verify open/click tracking
- Output: Provider swapped behind unified interface; no template changes needed; deliverability verified
