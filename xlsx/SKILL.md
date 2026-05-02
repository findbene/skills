---
name: xlsx
description: "Spreadsheet file operations for creating, reading, editing, and manipulating Excel (XLSX) and CSV files using Python. Use this skill any time spreadsheet files need to be created or modified, Excel data needs to be processed, CSV files need to be manipulated, or data needs to be exported to spreadsheet format. Trigger immediately on: \".xlsx\", \".csv\", \"Excel\", \"spreadsheet\", \"Excel file\", \"CSV file\", \"XLSX\", \"create spreadsheet\", \"edit Excel\", \"data to Excel\", \"parse CSV\", \"Excel report\", \"export to Excel\", \"Google Sheets format\". If someone shares an .xlsx or .csv file or asks to create a spreadsheet this skill MUST trigger."
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
