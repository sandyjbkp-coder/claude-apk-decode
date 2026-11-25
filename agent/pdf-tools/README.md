# PDF Tools — AndroidHarness

Complete PDF toolset installed in Alpine Linux chroot at `/data/local/tmp/moto-linux-env/rootfs/`

## Installed Tools

| Tool | Version | Package | Purpose |
|------|---------|---------|---------|
| qpdf | 12.3.2 | qpdf | PDF manipulation, linearization, encryption |
| ghostscript | 10.08.0 | ghostscript | PDF rendering, conversion, optimization |
| pdftotext | 25.12.0 | poppler-utils | Extract text from PDF |
| pdfinfo | 25.12.0 | poppler-utils | PDF metadata |
| pdfunite | 25.12.0 | poppler-utils | Merge PDFs |
| pdfseparate | 25.12.0 | poppler-utils | Split PDF into pages |
| pdftoppm | 25.12.0 | poppler-utils | Convert PDF to images |
| pdftohtml | 25.12.0 | poppler-utils | Convert PDF to HTML |
| convert (magick) | 7.1.2-35 | imagemagick | Image manipulation, PDF creation |
| tesseract | 5.5.2 | tesseract-ocr | OCR for scanned PDFs/images |

## Missing Tools

| Tool | Reason |
|------|--------|
| pdftk | Not available in Alpine 3.24 repos |

## Usage

All tools run inside the chroot:
```bash
chroot /data/local/tmp/moto-linux-env/rootfs /bin/ash -c "PATH=/usr/sbin:/usr/bin:/sbin:/bin <tool> <args>"
```

## Test Results

See `test-pdf.md` for complete test run with commands, exit codes, and output paths.
