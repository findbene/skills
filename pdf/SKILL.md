---
name: pdf
description: Read, create, merge, split, rotate, watermark, encrypt, extract text/tables from PDF files using Python. Trigger immediately any time a user mentions ".pdf", "PDF", "read this PDF", "merge PDFs", "split PDF", "extract from PDF", "PDF form", "combine PDFs", "PDF pages", "PDF to text", "watermark PDF", "compress PDF", "convert to PDF", "PDF password", "scanned PDF", or shares a .pdf file. Also trigger when the user asks to process, analyze, or generate any document that should be output as a PDF. Do not wait for the user to explicitly say "use python-docx" or name a library — if the task involves a PDF, this skill applies.
---

# PDF Processing

Handle PDF operations — reading, creating, manipulating, and extracting content from PDF files.

The right tool depends on what you need to do. PDFs are not a single format — they can be text-based (selectable text), image-based (scanned, needs OCR), or form-based. Pick your library based on the task, not habit.

---

## Library Selection Guide

| Task | Best Tool | Why |
|------|-----------|-----|
| Extract text from text-based PDF | `pdfplumber` | Handles layout, columns, preserves whitespace |
| Extract tables | `pdfplumber` | Built-in table detection |
| Merge / split / rotate pages | `pypdf` | Lightweight, no dependencies |
| Create a new PDF from scratch | `reportlab` | Full typographic control |
| OCR scanned/image-based PDFs | `pytesseract` + `pdf2image` | Only option for image PDFs |
| Password-protect or decrypt | `pypdf` | Encryption built in |
| Watermarking | `pypdf` | Overlay pages together |
| Complex CLI operations | `qpdf` | Fast, reliable binary |

Install what you need:
```bash
pip install pypdf pdfplumber reportlab pdf2image pytesseract
brew install qpdf tesseract   # or apt-get on Linux
```

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

## CLI operations (qpdf)

```bash
# Merge
qpdf --empty --pages part1.pdf part2.pdf -- merged.pdf

# Extract pages 3-7
qpdf --pages input.pdf 3-7 -- output.pdf

# Decrypt
qpdf --decrypt --password=secret encrypted.pdf decrypted.pdf

# Compress
qpdf --recompress-flate --compression-level=9 input.pdf compressed.pdf
```

---

## Gotchas

- **`extract_text()` returns None**: the PDF is image-based. Use OCR.
- **Garbled text**: likely a character encoding issue or a scanned PDF with embedded fonts. Try `pdfplumber` before concluding OCR is needed.
- **ReportLab font limits**: built-in fonts (Helvetica, Times-Roman, Courier) don't support Unicode beyond Latin-1. Register a TTF font for full Unicode support.
- **pypdf vs PyPDF2**: `pypdf` is the maintained successor to `PyPDF2`. Always use `pypdf`.
- **Page rotation vs. content rotation**: rotating a page in pypdf rotates the viewport, not the content. For scanned images inside PDFs, you may need to rotate the image itself before embedding.
