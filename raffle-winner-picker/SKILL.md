---
name: raffle-winner-picker
description: "Picks random winners from lists, spreadsheets, or Google Sheets for giveaways, raffles, and contests. Trigger: pick a random winner, run a raffle, select someone from this list, who wins the giveaway"
allowed-tools: Glob, Grep, Read
---

# Raffle Winner Picker

This skill randomly selects winners from lists, spreadsheets, or Google Sheets for giveaways and contests.

## When to Use This Skill

- Running social media giveaways
- Picking raffle winners at events
- Randomly selecting participants for surveys or tests
- Choosing winners from contest submissions
- Fair distribution of limited spots or resources
- Random team assignments

## What This Skill Does

1. **Random Selection**: Uses cryptographically random selection → verify: step output matches expected outcome
2. **Multiple Sources**: Works with CSV, Excel, Google Sheets, or plain lists → verify: step output matches expected outcome
3. **Multiple Winners**: Can pick one or multiple winners → verify: step output matches expected outcome
4. **Duplicate Prevention**: Ensures the same person can't win twice → verify: step output matches expected outcome
5. **Transparent Results**: Shows the selection process clearly → verify: step output matches expected outcome
6. **Winner Details**: Displays all relevant information about winners → verify: step output matches expected outcome

## How to Use

### From Google Sheets

```
Pick a random row from this Google Sheet to select a winner 
for a giveaway: [Sheet URL]
```

### From Local File

```
Pick 3 random winners from entries.csv
```

### From List

```
Pick a random winner from this list:
- Alice (alice@email.com)
- Bob (bob@email.com)
- Carol (carol@email.com)
...
```

### Multiple Winners

```
Pick 5 random winners from contest-entries.xlsx, 
make sure no duplicates
```

## Example

**User**: "Pick a random row from this Google Sheet to select a winner for a giveaway."

**Output**:
```
Accessing Google Sheet...
Total entries found: 247

Randomly selecting winner...

🎉 WINNER SELECTED! 🎉

Row #142
Name: Sarah Johnson
Email: sarah.j@email.com
Entry Date: March 10, 2024
Comment: "Love your newsletter!"

Selection method: Cryptographically random
Timestamp: 2024-03-15 14:32:18 UTC

Would you like to:
- Pick another winner (excluding Sarah)?
- Export winner details?
- Pick runner-ups?
```

**Inspired by:** Lenny's use case - picking a Sora 2 giveaway winner from his subscriber Slack community

## Features

### Fair Selection
- Uses secure random number generation
- No bias or patterns
- Transparent process
- Repeatable with seed (for verification)

### Exclusions
```
Pick a random winner excluding previous winners: 
Alice, Bob, Carol
```

### Weighted Selection
```
Pick a winner with weighted probability based on 
the "entries" column (1 entry = 1 ticket)
```

### Runner-ups
```
Pick 1 winner and 3 runner-ups from the list
```

## Example Workflows

### Social Media Giveaway
1. Export entries from Google Form to Sheets → verify: step output matches expected outcome
2. "Pick a random winner from [Sheet URL]" → verify: step output matches expected outcome
3. Verify winner details
4. Announce publicly with timestamp → verify: step output matches expected outcome

### Event Raffle
1. Create CSV of attendee names and emails → verify: output file exists + no syntax error
2. "Pick 10 random winners from attendees.csv" → verify: step output matches expected outcome
3. Export winner list → verify: step output matches expected outcome
4. Email winners directly → verify: step output matches expected outcome

### Team Assignment
1. Have list of participants → verify: step output matches expected outcome
2. "Randomly split this list into 4 equal teams" → verify: step output matches expected outcome
3. Review assignments → verify: step output matches expected outcome
4. Share team rosters → verify: step output matches expected outcome

## Tips

- **Document the process**: Save the timestamp and method
- **Public announcement**: Share selection details for transparency
- **Check eligibility**: Verify winner meets contest rules
- **Have backups**: Pick runner-ups in case winner is ineligible
- **Export results**: Save winner list for records

## Privacy & Fairness

✓ Uses cryptographically secure randomness
✓ No manipulation possible
✓ Timestamp recorded for verification
✓ Can provide seed for third-party verification
✓ Respects data privacy

## Common Use Cases

- Newsletter subscriber giveaways
- Product launch raffles
- Conference ticket drawings
- Beta tester selection
- Focus group participant selection
- Random prize distribution at events

## When NOT to use

- Task is unrelated to raffle winner picker — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Raffle Winner Picker needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for raffle winner picker
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow
