# PDF Tools — AndroidHarness (v1.0.0)

## Installation Status

| Tool | Version | Package | Status | Notes |
|------|---------|---------|--------|-------|
| qpdf | 12.3.2 | qpdf | ✅ INSTALLED | PDF manipulation, linearization, encryption |
| ghostscript | 10.08.0 | ghostscript | ✅ INSTALLED | PDF rendering, conversion, optimization |
| pdftotext | 25.12.0 | poppler-utils | ✅ INSTALLED | Text extraction |
| pdfinfo | 25.12.0 | poppler-utils | ✅ INSTALLED | PDF metadata |
| pdfunite | 25.12.0 | poppler-utils | ✅ INSTALLED | PDF merging |
| pdfseparate | 25.12.0 | poppler-utils | ✅ INSTALLED | PDF splitting |
| pdftoppm | 25.12.0 | poppler-utils | ✅ INSTALLED | PDF to image conversion |
| pdftohtml | 25.12.0 | poppler-utils | ✅ INSTALLED | PDF to HTML |
| convert (ImageMagick) | 7.1.2-35 | imagemagick | ✅ INSTALLED | Image manipulation, PDF creation |
| tesseract-ocr | 5.5.2 | tesseract-ocr | ✅ INSTALLED | OCR for scanned documents |
| tesseract-ocr-data-eng | 5.5.2 | tesseract-ocr-data-eng | ✅ INSTALLED | English language data |
| font-dejavu | 2.37-r6 | font-dejavu | ✅ INSTALLED | Font support for PDF rendering |

## Missing Tool

| Tool | Status | Reason |
|------|--------|--------|
| pdftk | ❌ MISSING | Not available in Alpine Linux repositories |

## Capabilities (All Supported)

- Create PDF from text (convert)
- Create PDF from images (convert)
- Merge PDFs (pdfunite)
- Split PDFs (pdfseparate)
- Extract text (pdftotext)
- Extract text with layout (pdftotext -layout)
- Convert PDF to images (pdftoppm)
- Convert images to PDF (convert)
- Extract text with layout (pdftotext)
- OCR scanned documents (tesseract)
- Add watermark (qpdf --overlay)
- Password protect PDF (qpdf --encrypt)
- Remove metadata (qpdf --empty --pages)
- Repair/linearize PDF (qpdf --linearize)
- Flatten forms/annotations (gs -dPrinted=false)
- Rotate pages (qpdf --rotate)
- Compress/optimize PDF (gs -dPDFSETTINGS=/ebook / qpdf --linearize)

## Usage

All tools require chroot execution:
```bash
chroot /data/local/tmp/moto-linux-env/rootfs /bin/ash -c "PATH=/usr/sbin:/usr/bin:/sbin:/bin <tool> <args>"
```

## Test Results

See `/agent/pdf-tools/test-pdf.md` for complete test results including:
- Tool version verification
- PDF creation and manipulation tests
- Exit codes and output paths
- All operations completed with exit code 0
