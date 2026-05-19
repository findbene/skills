---
name: xlsx
description: 'Spreadsheet file operations for creating, reading, editing, and manipulating Excel (XLSX) and CSV files using Python. Triggers: "use xlsx", "process spreadsheet", "xlsx".'
allowed-tools: Glob, Grep, Read
---

# XLSX Skill

Handle spreadsheet operations with professional standards.

## Financial Model Standards

**Color Coding:**
- Blue: Inputs
- Black: Formulas
- Green: Internal links
- Red: External links
- Yellow background: Key assumptions

**Formatting:**
- Years as text
- Currency with unit headers
- Zeros displayed as "-"
- Percentages at 0.0% format

**Critical Rule:** Always use Excel formulas instead of calculating values in Python and hardcoding them — maintain dynamic spreadsheets.

## Tools & Libraries

### pandas — Data Analysis
```python
import pandas as pd

df = pd.read_excel("data.xlsx", sheet_name="Sheet1")
df = pd.read_csv("data.csv")

df.to_excel("output.xlsx", index=False)
df.to_csv("output.csv", index=False)
```

### openpyxl — Complex Formatting
```python
from openpyxl import Workbook, load_workbook
from openpyxl.styles import Font, PatternFill

wb = load_workbook("template.xlsx")
ws = wb.active

ws['A1'].font = Font(bold=True, color="0000FF")
ws['A1'].fill = PatternFill(start_color="FFFF00", fill_type="solid")
ws['C1'] = "=SUM(A1:B1)"

wb.save("output.xlsx")
```

## Formula Error Prevention

- Deliver files with ZERO formula errors
- Verify all cell references
- Test edge cases
- Check for: #REF!, #DIV/0!, #VALUE!, #N/A, #NAME?

## Best Practices

- All assumptions in separate cells with formula references, never hardcoded
- Document sources for hardcoded data with specific citations
- Preserve existing template conventions
- Use professional fonts (Arial, Times New Roman)
- Maintain consistency across deliverables

## When NOT to use

- Task is unrelated to xlsx — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Xlsx needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for xlsx
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving xlsx
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
