# Extended Details

## Library Selection

| Situation | Tool |
|-----------|------|
| Creating a presentation from scratch in Node.js | `pptxgenjs` |
| Creating or editing a presentation in Python | `python-pptx` |
| Reading / extracting text from an existing .pptx | `markitdown` or `python-pptx` |
| Converting .pptx to PDF | LibreOffice headless |
| Converting slides to images | LibreOffice + `pdftoppm` |
| Editing raw XML when the library can't reach it | Unzip → edit XML → repack |

```bash
npm install -g pptxgenjs         # Node.js
pip install python-pptx          # Python
pip install "markitdown[pptx]"   # Reading
```

---

## Reading a PPTX

```bash
# Extract all text (simplest)
python -m markitdown presentation.pptx

# Raw XML access
unzip presentation.pptx -d unpacked/
# Slide XML: unpacked/ppt/slides/slide1.xml, slide2.xml, ...
```

```python
# python-pptx: iterate slides and shapes
from pptx import Presentation

prs = Presentation("presentation.pptx")
for i, slide in enumerate(prs.slides):
    print(f"\n--- Slide {i+1} ---")
    for shape in slide.shapes:
        if shape.has_text_frame:
            for para in shape.text_frame.paragraphs:
                print(para.text)
```

---

## Creating a Presentation (Node.js — pptxgenjs)

```javascript
const pptxgen = require("pptxgenjs");
const pptx = new pptxgen();

// Slide 1: Title slide
let slide1 = pptx.addSlide();
slide1.background = { color: "1E2761" };

slide1.addText("Presentation Title", {
  x: 0.5, y: 1.5, w: "90%", h: 1.5,
  fontSize: 44, bold: true, color: "FFFFFF",
  align: "center",
});

slide1.addText("Subtitle or tagline", {
  x: 0.5, y: 3.2, w: "90%",
  fontSize: 20, color: "CADCFC",
  align: "center",
});

// Slide 2: Content slide
let slide2 = pptx.addSlide();
slide2.addText("Section Title", {
  x: 0.5, y: 0.3, w: "90%",
  fontSize: 28, bold: true, color: "1E2761",
});

// Bullet points
slide2.addText([
  { text: "First key point", options: { bullet: true, fontSize: 16 } },
  { text: "Second key point", options: { bullet: true, fontSize: 16 } },
  { text: "Third key point", options: { bullet: true, fontSize: 16 } },
], { x: 0.5, y: 1.2, w: 5.5, h: 3 });

// Image
slide2.addImage({ path: "chart.png", x: 6.2, y: 1.2, w: 3.2, h: 3 });

pptx.writeFile({ fileName: "output.pptx" });
```

### Tables in pptxgenjs

```javascript
let slide = pptx.addSlide();
const rows = [
  [{ text: "Metric", options: { bold: true, fill: "1E2761", color: "FFFFFF" } },
   { text: "Value",  options: { bold: true, fill: "1E2761", color: "FFFFFF" } }],
  ["Revenue",  "$1.2M"],
  ["Growth",   "+34%"],
];

slide.addTable(rows, {
  x: 0.5, y: 1.0, w: 9.0,
  border: { type: "solid", color: "CCCCCC", pt: 1 },
  rowH: 0.5,
  fontSize: 14,
});
```

---

## Creating a Presentation (Python — python-pptx)

```python
from pptx import Presentation
from pptx.util import Inches, Pt, Emu
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN

prs = Presentation()
prs.slide_width = Inches(13.33)
prs.slide_height = Inches(7.5)

# Blank slide layout
blank_layout = prs.slide_layouts[6]
slide = prs.slides.add_slide(blank_layout)

# Add a text box
txBox = slide.shapes.add_textbox(Inches(0.5), Inches(0.5), Inches(12), Inches(1.5))
tf = txBox.text_frame
tf.word_wrap = True

p = tf.paragraphs[0]
p.text = "Slide Title"
p.font.size = Pt(36)
p.font.bold = True
p.font.color.rgb = RGBColor(0x1E, 0x27, 0x61)

# Add image
slide.shapes.add_picture("chart.png", Inches(7), Inches(2), width=Inches(5.5))

prs.save("output.pptx")
```

---

## Design System

Good slide design is the difference between a deck people read and one they close. These rules apply regardless of library.

### Palette strategy

Pick one primary color and build around it. Never default to blue just because it's PowerPoint's default — choose colors that fit the content's tone.

| Theme | Primary | Secondary | Accent |
|-------|---------|-----------|--------|
| Executive / Corporate | `#1E2761` | `#CADCFC` | `#FFFFFF` |
| Growth / Energy | `#F96167` | `#F9E795` | `#2F3C7E` |
| Trust / Healthcare | `#028090` | `#00A896` | `#F5F5F5` |
| Natural / Sustainable | `#2C5F2D` | `#97BC62` | `#F5F5F5` |

Use one color for 60–70% of visual weight. Dark backgrounds for title/conclusion slides; light for content slides.

### Typography

| Element | Size |
|---------|------|
| Slide title | 32–44pt bold |
| Section header | 20–24pt bold |
| Body text | 14–16pt |
| Captions / footnotes | 10–12pt, muted color |

Left-align body text. Center only titles on title slides.

### Layout variety

Vary the layout across slides — a uniform grid of text boxes feels like a template, not a deck. Rotate through:
- Two-column (text left, visual right)
- Full-bleed image with text overlay
- 2×2 or 3×3 stat/icon grid
- Single big statement centered
- Timeline or process flow

### What to avoid

- **Accent lines under titles** — the single clearest marker of an AI-generated deck. Never do this.
- **Centered body text** — reserve centering for titles only
- **Text-only slides** — every content slide should have a visual element
- **Same layout repeated** — rotate through at least 3 layout types
- **Default blue/white palette** — pick a palette intentional to the topic
- **More than 5–6 bullet points per slide** — split or cut

---

## Conversion

```bash
# PPTX → PDF
libreoffice --headless --convert-to pdf presentation.pptx

# PDF → images (one per slide)
pdftoppm -r 150 presentation.pdf slides/slide

# PPTX → images directly (LibreOffice)
libreoffice --headless --convert-to png presentation.pptx
```

---

## Editing an existing PPTX (XML approach)

When the library can't reach what you need:

```bash
# Unpack
unzip presentation.pptx -d unpacked/

# Slides: unpacked/ppt/slides/slide1.xml, slide2.xml ...
# Slide layout: unpacked/ppt/slideLayouts/
# Theme: unpacked/ppt/theme/theme1.xml

# Repack from inside the directory
cd unpacked && zip -r ../modified.pptx . && cd ..
```

---

## Gotchas

- **pptxgenjs coordinates**: `x`, `y`, `w`, `h` are in inches by default. Use `{ x: 0.5, y: 0.5, w: 9.0, h: 1.5 }` — not pixels.
- **python-pptx slide size**: default is 10×7.5 inches (4:3). For 16:9 widescreen: `Inches(13.33)` × `Inches(7.5)`.
- **Font not rendering**: pptxgenjs embeds fonts by name — the font must be available on the system rendering the file. Stick to web-safe fonts (`Arial`, `Calibri`, `Georgia`) unless embedding.
- **Images blurry**: provide source images at 2× the display size or set a high DPI. For a 5-inch-wide image, use a source image at least 960px wide at 96dpi.
- **Repacking PPTX manually**: the `zip` command must be run from inside the unpacked directory, not from the parent. `zip -r ../output.pptx .` from inside, not `zip -r output.pptx unpacked/`.
- **markitdown text extraction misses speaker notes**: access notes via `slide.notes_slide.notes_text_frame.text` in python-pptx.
