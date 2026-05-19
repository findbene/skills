# Extended Details

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
