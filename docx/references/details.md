# Extended Details

## Library Selection

| Situation | Tool |
|-----------|------|
| Creating or editing a document in Python | `python-docx` |
| Creating a document in Node.js | `docx` npm package |
| Reading/converting an existing .docx to text or markdown | `pandoc` |
| Editing raw structure when python-docx can't reach something | Unzip → edit XML → repack |

```bash
pip install python-docx          # Python
npm install docx                 # Node.js
brew install pandoc              # or apt-get
```

---

## Reading a DOCX

### Extract text (pandoc — simplest)

```bash
pandoc document.docx -t plain -o output.txt
# Or convert to markdown:
pandoc document.docx -t markdown -o output.md
```

### Extract text programmatically (python-docx)

```python
from docx import Document

doc = Document("document.docx")
for para in doc.paragraphs:
    print(para.text)

# Tables
for table in doc.tables:
    for row in table.rows:
        for cell in row.cells:
            print(cell.text, end="\t")
        print()
```

---

## Creating a DOCX (Python — python-docx)

### Basic document

```python
from docx import Document
from docx.shared import Pt, RGBColor, Inches
from docx.enum.text import WD_ALIGN_PARAGRAPH

doc = Document()

# Title
doc.add_heading("Document Title", level=0)

# Paragraph with formatting
para = doc.add_paragraph()
run = para.add_run("Bold and colored text")
run.bold = True
run.font.size = Pt(12)
run.font.color.rgb = RGBColor(0x1F, 0x49, 0x7D)

# Body paragraph
doc.add_paragraph("Regular body text goes here.")

doc.save("output.docx")
```

### Headings (use built-in styles — don't fake them with bold)

```python
doc.add_heading("Chapter 1", level=1)    # H1
doc.add_heading("Section 1.1", level=2)  # H2
doc.add_heading("Subsection", level=3)   # H3
```

Using built-in heading styles ensures the document generates a proper navigation pane and table of contents in Word.

### Bullet and numbered lists

```python
# Bullets
doc.add_paragraph("First item", style="List Bullet")
doc.add_paragraph("Second item", style="List Bullet")

# Numbered
doc.add_paragraph("Step one", style="List Number")
doc.add_paragraph("Step two", style="List Number")
```

Never manually insert `•` or `1.` as text — always use the list styles. Manual characters break re-numbering and look different across Word versions.

### Tables

```python
from docx.shared import Inches
from docx.enum.table import WD_TABLE_ALIGNMENT

table = doc.add_table(rows=1, cols=3)
table.style = "Table Grid"

# Header row
hdr = table.rows[0].cells
hdr[0].text = "Name"
hdr[1].text = "Role"
hdr[2].text = "Status"

# Data rows
data = [("Alice", "Engineer", "Active"), ("Bob", "Designer", "Active")]
for name, role, status in data:
    row = table.add_row().cells
    row[0].text = name
    row[1].text = role
    row[2].text = status
```

### Images

```python
doc.add_picture("chart.png", width=Inches(5.5))
```

Always specify `width` or `height` — without it, images insert at native DPI and are often enormous.

### Page breaks and sections

```python
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

doc.add_page_break()

# New section with different orientation (landscape)
new_section = doc.add_section()
new_section.orientation = WD_ORIENT.LANDSCAPE
new_section.page_width, new_section.page_height = (
    new_section.page_height, new_section.page_width
)
```

### Headers and footers

```python
section = doc.sections[0]
header = section.header
header.paragraphs[0].text = "Company Confidential"

footer = section.footer
footer.paragraphs[0].text = "Page 1"  # Static; for dynamic page numbers use XML field
```

---

## Creating a DOCX (Node.js — docx npm)

Use this when you're already working in a Node.js/TypeScript project.

```javascript
const { Document, Packer, Paragraph, TextRun, HeadingLevel, Table, TableRow, TableCell } = require("docx");
const fs = require("fs");

const doc = new Document({
  sections: [{
    children: [
      new Paragraph({
        text: "Document Title",
        heading: HeadingLevel.TITLE,
      }),
      new Paragraph({
        children: [
          new TextRun({ text: "Bold text", bold: true }),
          new TextRun(" and normal text."),
        ],
      }),
    ],
  }],
});

Packer.toBuffer(doc).then(buffer => {
  fs.writeFileSync("output.docx", buffer);
});
```

### Critical rules for docx npm

- **Page size**: Default is A4. For US Letter: `{ width: 12240, height: 15840 }` (1,440 DXA = 1 inch)
- **Newlines**: Use separate `Paragraph` elements — `\n` inside `TextRun` does not create new paragraphs
- **Tables**: Require both `columnWidths` on the table AND `width` on each cell. Use `WidthType.DXA` (not percentage — breaks in Google Docs)
- **Smart quotes**: Use XML entities `&#x201C;` and `&#x201D;` rather than raw curly quotes
- **Images**: Require `type` parameter (`png`, `jpg`, `jpeg`, `gif`, `bmp`, `svg`) and alt text

---

## Editing an existing DOCX (XML approach)

When python-docx can't reach what you need to change (e.g., SmartArt, complex custom XML):

```bash
# Unpack
unzip document.docx -d extracted/

# Edit
# Main content: extracted/word/document.xml
# Styles: extracted/word/styles.xml
# Headers/footers: extracted/word/header1.xml, footer1.xml

# Repack (must be from inside the extracted directory)
cd extracted && zip -r ../modified.docx . && cd ..
```

Be careful: malformed XML will make Word refuse to open the file. Always validate changes with a Word or LibreOffice open before delivering.

---

## Conversion

```bash
# DOCX → PDF
libreoffice --headless --convert-to pdf document.docx

# DOCX → HTML
pandoc document.docx -o output.html

# HTML → DOCX
pandoc input.html -o output.docx
```

---

## Gotchas

- **`KeyError: 'Normal'`** when applying styles: the style must exist in the document's style table. Either apply styles by their internal name or load a template document that already has the style defined.
- **Image too large**: always pass `width=Inches(N)` to `add_picture()`.
- **Table cell width ignored**: in the docx npm package, you must set width on both the table (`columnWidths`) and each individual cell.
- **Bullets look wrong**: don't use text characters for bullets — use `"List Bullet"` style.
- **Headers/footers on all pages**: by default, all sections inherit the first section's header/footer unless you explicitly unlink them with `header.is_linked_to_previous = False`.
