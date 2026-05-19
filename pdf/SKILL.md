---
name: pdf
description: 'Read, create, merge, split, rotate, watermark, encrypt, extract text/tables from PDF files using Python. Triggers: "use pdf", "process PDF", "pdf".'
allowed-tools: Glob, Grep, Read
---

# PDF Processing

Handle PDF operations — reading, creating, manipulating, and extracting content from PDF files.

The right tool depends on what you need to do. PDFs are not a single format — they can be text-based (selectable text), image-based (scanned, needs OCR), or form-based. Pick your library based on the task, not habit.

---

## Common Workflows

### Extract text from a PDF

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for i, page in enumerate(pdf.pages):
        text = page.extract_text()
        if text:
            print(f"--- Page {i+1} ---")
            print(text)
```

Use `extract_text(layout=True)` to preserve column structure. If `extract_text()` returns `None` or garbage, the PDF is image-based — fall back to OCR.

### Extract tables

```python
import pdfplumber

with pdfplumber.open("report.pdf") as pdf:
    for page in pdf.pages:
        tables = page.extract_tables()
        for table in tables:
            for row in table:
                print(row)
```

### Merge PDFs

```python
from pypdf import PdfWriter

writer = PdfWriter()
for path in ["part1.pdf", "part2.pdf", "part3.pdf"]:
    writer.append(path)
writer.write("merged.pdf")
writer.close()
```

### Split PDF — extract specific pages

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()

# Extract pages 3–7 (0-indexed)
for i in range(2, 7):
    writer.add_page(reader.pages[i])

writer.write("extracted.pdf")
writer.close()
```

### Rotate pages

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()
for page in reader.pages:
    page.rotate(90)  # 90, 180, or 270
    writer.add_page(page)
writer.write("rotated.pdf")
writer.close()
```

### Add watermark

```python
from pypdf import PdfReader, PdfWriter

stamp = PdfReader("watermark.pdf")
watermark_page = stamp.pages[0]

reader = PdfReader("document.pdf")
writer = PdfWriter()
for page in reader.pages:
    page.merge_page(watermark_page)
    writer.add_page(page)
writer.write("watermarked.pdf")
writer.close()
```

### Password protect / encrypt

```python
from pypdf import PdfWriter

writer = PdfWriter()
writer.append("input.pdf")
writer.encrypt("user_password", "owner_password", use_128bit=True)
writer.write("protected.pdf")
writer.close()
```

### Create a PDF from scratch

```python
from reportlab.pdfgen import canvas
from reportlab.lib.pagesizes import letter, A4
from reportlab.lib.units import inch

c = canvas.Canvas("output.pdf", pagesize=letter)
width, height = letter

# Title
c.setFont("Helvetica-Bold", 18)
c.drawString(1 * inch, height - 1.5 * inch, "Report Title")

# Body text
c.setFont("Helvetica", 12)
c.drawString(1 * inch, height - 2.5 * inch, "Body text goes here.")

c.save()
```

Avoid Unicode subscript/superscript characters in ReportLab's built-in fonts — they render as boxes. Use `<sub>` and `<super>` XML tags with Paragraph markup instead.

For multi-page documents with flowing text, use `reportlab.platypus`:

```python
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer
from reportlab.lib.styles import getSampleStyleSheet

doc = SimpleDocTemplate("output.pdf", pagesize=letter)
styles = getSampleStyleSheet()
story = []

story.append(Paragraph("Title", styles["Title"]))
story.append(Spacer(1, 12))
story.append(Paragraph("Body paragraph text.", styles["Normal"]))

doc.build(story)
```

### OCR a scanned PDF

```python
from pdf2image import convert_from_path
import pytesseract

pages = convert_from_path("scanned.pdf", dpi=300)
full_text = ""
for page in pages:
    full_text += pytesseract.image_to_string(page)
print(full_text)
```

Higher DPI (300+) produces better OCR results at the cost of speed.

---

## When NOT to use

- Task is unrelated to pdf — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Pdf needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for pdf
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving pdf
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
