# PDF Capabilities — AndroidHarness

Documented capabilities using installed PDF tools.

## Tool Mapping to Operations

| Operation | Tool | Command Example |
|-----------|------|-----------------|
| **Create PDF from text** | `convert` (IM) + `pandoc` (if available) | `echo "Hello" | convert - test.pdf` |
| **Create PDF from Markdown** | `pandoc` (not installed) / `weasyprint` (Python) | `pandoc -o out.pdf in.md` |
| **Create PDF from HTML** | `weasyprint` / `convert` / `gs` | `weasyprint in.html out.pdf` |
| **Create PDF from images** | `convert` / `img2pdf` (Python) | `convert *.png out.pdf` |
| **Merge multiple PDFs** | `pdfunite` / `qpdf` | `pdfunite in1.pdf in2.pdf out.pdf` |
| **Split PDF into pages** | `pdfseparate` | `pdfseparate in.pdf out-%d.pdf` |
| **Extract selected pages** | `qpdf` / `pdftk` (missing) | `qpdf --empty --pages in.pdf 1-3 -- out.pdf` |
| **Rotate pages** | `qpdf` / `pdftk` (missing) | `qpdf --rotate=90:1 in.pdf out.pdf` |
| **Compress/optimize PDF** | `gs` / `qpdf` | `gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -o out.pdf in.pdf` |
| **Convert PDF to text** | `pdftotext` | `pdftotext in.pdf out.txt` |
| **Convert PDF to images** | `pdftoppm` / `convert` | `pdftoppm -png in.pdf page` |
| **Convert images to PDF** | `convert` / `img2pdf` | `convert *.png out.pdf` |
| **Extract text with layout** | `pdftotext -layout` | `pdftotext -layout in.pdf out.txt` |
| **OCR scanned PDF/images** | `tesseract` / `ocrmypdf` (not installed) | `tesseract in.png out -l eng pdf` |
| **Add watermark** | `qpdf` / `gs` / `pdftk` (missing) | `qpdf --overlay watermark.pdf -- in.pdf out.pdf` |
| **Password protect PDF** | `qpdf` | `qpdf --encrypt userpass ownerpass 256 -- in.pdf out.pdf` |
| **Remove metadata** | `qpdf` / `exiftool` (not installed) | `qpdf --empty --pages in.pdf 1-z -- out.pdf` |
| **Repair/linearize PDF** | `qpdf` | `qpdf --linearize in.pdf out.pdf` |
| **Flatten forms/annotations** | `gs` / `qpdf` | `gs -o out.pdf -sDEVICE=pdfwrite -dPrinted=false in.pdf` |
| **Convert DOCX to PDF** | `libreoffice` (not installed) / `pandoc` | `libreoffice --headless --convert-to pdf in.docx` |
| **Convert HTML to PDF** | `weasyprint` / `wkhtmltopdf` (not installed) | `weasyprint in.html out.pdf` |

## Available Operations (✅ = Working, ❌ = Missing Tool, ⚠️ = Partial)

| Capability | Status | Notes |
|------------|--------|-------|
| Create PDF from text | ✅ | `convert` (ImageMagick) |
| Create PDF from Markdown | ❌ | Need `pandoc` or `weasyprint` (Python install failed) |
| Create PDF from HTML | ❌ | Need `weasyprint` / `wkhtmltopdf` |
| Create PDF from images | ✅ | `convert *.png out.pdf` |
| Merge PDFs | ✅ | `pdfunite` / `qpdf` |
| Split PDF | ✅ | `pdfseparate` |
| Extract pages | ✅ | `qpdf --pages` |
| Rotate pages | ✅ | `qpdf --rotate` |
| Compress/optimize | ✅ | `gs -dPDFSETTINGS=/ebook` / `qpdf --linearize` |
| PDF to text | ✅ | `pdftotext` / `pdftotext -layout` |
| PDF to images | ✅ | `pdftoppm -png` / `convert` |
| Images to PDF | ✅ | `convert` |
| Extract text with layout | ✅ | `pdftotext -layout` |
| OCR | ✅ | `tesseract` (manual), `ocrmypdf` not installed |
| Add watermark | ✅ | `qpdf --overlay` (needs watermark PDF) |
| Password protect | ✅ | `qpdf --encrypt` |
| Remove metadata | ✅ | `qpdf --empty --pages` |
| Repair/linearize | ✅ | `qpdf --linearize` |
| Flatten forms | ✅ | `gs -dPrinted=false` |
| DOCX to PDF | ❌ | Need `libreoffice` / `pandoc` |
| HTML to PDF | ❌ | Need `weasyprint` / `wkhtmltopdf` |

## Python PDF Libraries (Not Yet Installed)

| Library | Purpose | Install Command |
|---------|---------|-----------------|
| pypdf / PyPDF2 | PDF manipulation | `pip install pypdf` |
| pdfplumber | Text/table extraction | `pip install pdfplumber` |
| pymupdf (fitz) | Fast PDF processing | `pip install pymupdf` |
| weasyprint | HTML to PDF | `pip install weasyprint` |
| reportlab | PDF generation | `pip install reportlab` |
| img2pdf | Images to PDF | `pip install img2pdf` |
| ocrmypdf | OCR pipeline | `pip install ocrmypdf` |

## Chroot Execution Pattern

All tools require chroot:
```bash
CHROOT="chroot /data/local/tmp/moto-linux-env/rootfs /bin/ash -c"
PATH='PATH=/usr/sbin:/usr/bin:/sbin:/bin'
$CHROOT "$PATH <tool> <args>"
```

## pdftk Alternative

Since `pdftk` is not in Alpine repos, use `qpdf` for:
- Page extraction: `qpdf --empty --pages in.pdf 1-3 -- out.pdf`
- Page rotation: `qpdf --rotate=90:1 in.pdf out.pdf`
- Page reordering: `qpdf --empty --pages in.pdf 3,1,2 -- out.pdf`
- Stamping/watermark: `qpdf --overlay=wm.pdf -- in.pdf out.pdf`
