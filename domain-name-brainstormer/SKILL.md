---
name: domain-name-brainstormer
description: 'Generates creative domain name ideas for your project and checks availability across multiple TLDs (.com, .io, .dev. Triggers: "use domain-name-brainstormer", "domain name brainstormer", "domain task.'
allowed-tools: Glob, Grep, Read
---

# Domain Name Brainstormer

This skill helps you find the perfect domain name for your project by generating creative options and checking what's actually available to register.

## When to Use This Skill

- Starting a new project or company
- Launching a product or service
- Creating a personal brand or portfolio site
- Rebranding an existing project
- Registering a domain for a side project
- Finding available alternatives when your first choice is taken

## How to Use

### Basic Brainstorming

```
I'm building a project management tool for remote teams. 
Suggest domain names.
```

```
Help me brainstorm domain names for a personal finance app
```

### Specific Preferences

```
I need a domain name for my AI writing assistant. 
Prefer short names with .ai or .io extension.
```

### With Keywords

```
Suggest domain names using the words "pixel" or "studio" 
for my design agency
```

## Example Workflows

### Startup Launch
1. Describe your startup idea → verify: step output matches expected outcome
2. Get 10-15 domain suggestions across TLDs → verify: step output matches expected outcome
3. Review availability and pricing → verify: step output matches expected outcome
4. Pick top 3 favorites → verify: step output matches expected outcome
5. Register immediately → verify: step output matches expected outcome

### Personal Brand
1. Share your name and profession → verify: step output matches expected outcome
2. Get variations (firstname.com, firstnamelastname.dev, etc.) → verify: step output matches expected outcome
3. Check social media handle availability too → verify: all tests pass
4. Register consistent brand across platforms → verify: step output matches expected outcome

### Product Naming
1. Describe product and target market → verify: step output matches expected outcome
2. Get creative, brandable names → verify: step output matches expected outcome
3. Check trademark conflicts → verify: all tests pass
4. Verify domain and social availability
5. Test names with target audience → verify: all tests pass

## When NOT to use

- Task is unrelated to domain name brainstormer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Domain Name Brainstormer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for domain name brainstormer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## References

Extended sections moved to `references/details.md`.
