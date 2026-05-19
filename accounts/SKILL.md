---
name: accounts
description: "Manage TikTok profile accounts for the video-transcriber scraping workflow. Triggers: 'use accounts', 'accounts', 'accounts task'."
allowed-tools: Glob, Grep, Read
user_invocable: true
---

# Accounts - Manage TikTok Profiles

This command lets users manage which TikTok profiles they want to scrape from.

## Workflow

### Step 1: Show Options

Display:

---

**Manage TikTok Profiles**

What would you like to do?

1. **Switch profile** - Change to a different TikTok account → verify: step output matches expected outcome
2. **Add profile** - Add another profile to your list → verify: package installed + import succeeds
3. **List profiles** - See all saved profiles → verify: step output matches expected outcome
4. **Remove profile** - Remove a profile from your list → verify: step output matches expected outcome
5. **Process multiple** - Scrape from multiple profiles at once → verify: step output matches expected outcome

---

### Step 2: Handle User Choice

#### Option 1: Switch Profile

Ask:
> What TikTok profile URL do you want to switch to?
>
> Example: `https://www.tiktok.com/@newusername`

Then:
1. Validate the URL format → verify: step output matches expected outcome
2. Test that the profile exists (quick yt-dlp check) → verify: all tests pass
3. Save to `accounts.json` as the active profile → verify: step output matches expected outcome
4. Confirm the switch → verify: step output matches expected outcome

#### Option 2: Add Profile

Ask:
> What TikTok profile URL do you want to add?
>
> Example: `https://www.tiktok.com/@anotherusername`

Then:
1. Validate the URL → verify: step output matches expected outcome
2. Add to `accounts.json` profiles list → verify: package installed + import succeeds
3. Confirm addition → verify: package installed + import succeeds

#### Option 3: List Profiles

Read `accounts.json` and display:

```
Saved Profiles:
---------------
1. @example_creator (active) → verify: step output matches expected outcome
   https://www.tiktok.com/@example_creator

2. @techcreator → verify: step output matches expected outcome
   https://www.tiktok.com/@techcreator

3. @aiexplainer → verify: step output matches expected outcome
   https://www.tiktok.com/@aiexplainer
```

#### Option 4: Remove Profile

Show list and ask which to remove.

#### Option 5: Process Multiple

Ask:
> Which profiles do you want to process? (comma-separated numbers or "all")

Then run `/bulk` for each selected profile sequentially.

---

## Triggers

add account, switch profile, add TikTok account, list my profiles, manage accounts, switch to a different TikTok, process multiple profiles, which account am I using, add another creator, remove a profile

## When NOT to use

- Task is unrelated to accounts — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Accounts needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for accounts
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## References

Extended sections moved to `references/details.md`.
