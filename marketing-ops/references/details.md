# Extended Details

## Before Starting

**Check for marketing context first:**
If `marketing-context.md` exists, read it. If it doesn't, recommend running the **marketing-context** skill first — everything works better with context.

## How This Skill Works

### Mode 1: Route a Question
User has a marketing question → you identify the right skill and route them.

### Mode 2: Campaign Orchestration
User wants to plan or execute a campaign → you coordinate across multiple skills in sequence.

### Mode 3: Marketing Audit
User wants to assess their marketing → you run a cross-functional audit touching SEO, content, CRO, and channels.

---

## Routing Matrix

### Content Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "Write a blog post," "content ideas," "what should I write" | **content-strategy** | Not copywriting (that's for page copy) |
| "Write copy for my homepage," "landing page copy," "headline" | **copywriting** | Not content-strategy (that's for planning) |
| "Edit this copy," "proofread," "polish this" | **copy-editing** | Not copywriting (that's for writing new) |
| "Social media post," "LinkedIn post," "tweet" | **social-content** | Not social-media-manager (that's for strategy) |
| "Marketing ideas," "brainstorm," "what else can I try" | **marketing-ideas** | |
| "Write an article," "research and write," "SEO article" | **content-production** | Not content-creator (production has the full pipeline) |
| "Sounds too robotic," "make it human," "AI watermarks" | **content-humanizer** | |

### SEO Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "SEO audit," "technical SEO," "on-page SEO" | **seo-audit** | Not ai-seo (that's for AI search engines) |
| "AI search," "ChatGPT visibility," "Perplexity," "AEO" | **ai-seo** | Not seo-audit (that's traditional SEO) |
| "Schema markup," "structured data," "JSON-LD," "rich snippets" | **schema-markup** | |
| "Site structure," "URL structure," "navigation," "sitemap" | **site-architecture** | |
| "Programmatic SEO," "pages at scale," "template pages" | **programmatic-seo** | |

### CRO Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "Optimize this page," "conversion rate," "CRO audit" | **page-cro** | Not form-cro (that's for forms specifically) |
| "Form optimization," "lead form," "contact form" | **form-cro** | Not signup-flow-cro (that's for registration) |
| "Signup flow," "registration," "account creation" | **signup-flow-cro** | Not onboarding-cro (that's post-signup) |
| "Onboarding," "activation," "first-run experience" | **onboarding-cro** | Not signup-flow-cro (that's pre-signup) |
| "Popup," "modal," "overlay," "exit intent" | **popup-cro** | |
| "Paywall," "upgrade screen," "upsell modal" | **paywall-upgrade-cro** | |

### Channels Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "Email sequence," "drip campaign," "welcome sequence" | **email-sequence** | Not cold-email (that's for outbound) |
| "Cold email," "outreach," "prospecting email" | **cold-email** | Not email-sequence (that's for lifecycle) |
| "Paid ads," "Google Ads," "Meta ads," "ad campaign" | **paid-ads** | Not ad-creative (that's for copy generation) |
| "Ad copy," "ad headlines," "ad variations," "RSA" | **ad-creative** | Not paid-ads (that's for strategy) |
| "Social media strategy," "social calendar," "community" | **social-media-manager** | Not social-content (that's for individual posts) |

### Growth Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "A/B test," "experiment," "split test" | **ab-test-setup** | |
| "Referral program," "affiliate," "word of mouth" | **referral-program** | |
| "Free tool," "calculator," "marketing tool" | **free-tool-strategy** | |
| "Churn," "cancel flow," "dunning," "retention" | **churn-prevention** | |

### Intelligence Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "Campaign analytics," "channel performance," "attribution" | **campaign-analytics** | Not analytics-tracking (that's for setup) |
| "Set up tracking," "GA4," "GTM," "event tracking" | **analytics-tracking** | Not campaign-analytics (that's for analysis) |
| "Competitor page," "vs page," "alternative page" | **competitor-alternatives** | |
| "Psychology," "persuasion," "behavioral science" | **marketing-psychology** | |

### Sales & GTM Pod
| Trigger | Route to | NOT this |
|---------|----------|----------|
| "Product launch," "feature announcement," "Product Hunt" | **launch-strategy** | |
| "Pricing," "how much to charge," "pricing tiers" | **pricing-strategy** | |

### Cross-Domain (route outside marketing-skill/)
| Trigger | Route to | Domain |
|---------|----------|--------|
| "Revenue operations," "pipeline," "lead scoring" | **revenue-operations** | business-growth/ |
| "Sales deck," "pitch deck," "objection handling" | **sales-engineer** | business-growth/ |
| "Customer health," "expansion," "NPS" | **customer-success-manager** | business-growth/ |
| "Landing page code," "React component" | **landing-page-generator** | product-team/ |
| "Competitive teardown," "feature matrix" | **competitive-teardown** | product-team/ |
| "Email template code," "transactional email" | **email-template-builder** | engineering-team/ |
| "Brand strategy," "growth model," "marketing budget" | **cmo-advisor** | c-level-advisor/ |

---

## Campaign Orchestration

For multi-skill campaigns, follow this sequence:

### New Product/Feature Launch
```
1. marketing-context (ensure foundation exists) → verify: step output matches expected outcome
2. launch-strategy (plan the launch) → verify: step output matches expected outcome
3. content-strategy (plan content around launch) → verify: step output matches expected outcome
4. copywriting (write landing page) → verify: output exists + parses without error
5. email-sequence (write launch emails) → verify: output exists + parses without error
6. social-content (write social posts) → verify: output exists + parses without error
7. paid-ads + ad-creative (paid promotion) → verify: step output matches expected outcome
8. analytics-tracking (set up tracking) → verify: step output matches expected outcome
9. campaign-analytics (measure results) → verify: step output matches expected outcome
```

### Content Campaign
```
1. content-strategy (plan topics + calendar) → verify: step output matches expected outcome
2. seo-audit (identify SEO opportunities) → verify: findings count > 0 OR clean signal returned
3. content-production (research → write → optimize)
4. content-humanizer (polish for natural voice) → verify: step output matches expected outcome
5. schema-markup (add structured data) → verify: dependency resolves + import works
6. social-content (promote on social) → verify: step output matches expected outcome
7. email-sequence (distribute via email) → verify: step output matches expected outcome
```

### Conversion Optimization Sprint
```
1. page-cro (audit current pages) → verify: findings count > 0 OR clean signal returned
2. copywriting (rewrite underperforming copy) → verify: output exists + parses without error
3. form-cro or signup-flow-cro (optimize forms) → verify: step output matches expected outcome
4. ab-test-setup (design tests) → verify: all checks pass
5. analytics-tracking (ensure tracking is right) → verify: step output matches expected outcome
6. campaign-analytics (measure impact) → verify: step output matches expected outcome
```

---

## Quality Gate

Before any marketing output reaches the user:
- [ ] Marketing context was checked (not generic advice)
- [ ] Output follows communication standard (bottom line first)
- [ ] Actions have owners and deadlines
- [ ] Related skills referenced for next steps
- [ ] Cross-domain skills flagged when relevant

---

## Output Artifacts

| When you ask for... | You get... |
|---------------------|------------|
| "What marketing skill should I use?" | Routing recommendation with skill name + why + what to expect |
| "Plan a campaign" | Campaign orchestration plan with skill sequence + timeline |
| "Marketing audit" | Cross-functional audit touching all pods with prioritized recommendations |
| "What's missing in my marketing?" | Gap analysis against full skill ecosystem |

## Communication

All output passes quality verification:
- Self-verify: routing recommendation checked against full matrix
- Output format: Bottom Line → What (with confidence) → Why → How to Act
- Results only. Every finding tagged: 🟢 verified, 🟡 medium, 🔴 assumed.

## Related Skills

- **chief-of-staff** (C-Suite): The C-level router. Marketing-ops is the domain-specific equivalent.
- **marketing-context**: Foundation — run this first if it doesn't exist.
- **cmo-advisor** (C-Suite): Strategic marketing decisions. Marketing-ops handles execution routing.
- **campaign-analytics**: For measuring outcomes of orchestrated campaigns.
