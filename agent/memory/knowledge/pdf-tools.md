# PDF Tools Knowledge Base — Android Self-Healing Agent

- **Installed:** 2026-11-25
- **Chroot:** /data/local/tmp/moto-linux-env/rootfs
- **Status:** READY
- **Missing:** pdftk (not in Alpine repos)

## Installed Tools

| Tool | Version | Package |
|------|---------|---------|
| qpdf | 12.3.2 | qpdf |
| ghostscript | 10.08.0 | ghostscript |
| pdftotext | 25.12.0 | poppler-utils |
| pdfinfo | 25.12.0 | poppler-utils |
| pdfunite | 25.12.0 | poppler-utils |
| pdfseparate | 25.12.0 | poppler-utils |
| pdftoppm | 25.12.0 | poppler-utils |
| pdftohtml | 25.12.0 | poppler-utils |
| convert (ImageMagick) | 7.1.2-35 | imagemagick |
| tesseract-ocr | 5.5.2 | tesseract-ocr |
| tesseract-ocr-data-eng | 5.5.2 | tesseract-ocr-data-eng |
| font-dejavu | 2.37-r6 | font-dejavu |

## Missing Tools

| Tool | Reason |
|------|--------|
| pdftk | Not available in Alpine Linux repositories |

## Usage Patterns

```bash
# Basic PDF operations
CHROOT="chroot /data/local/tmp/moto-linux-env/rootfs /bin/ash -c"
PATH="PATH=/usr/sbin:/usr/bin:/sbin:/bin"

# Create PDF
CHROOT "$PATH convert - test.pdf"

# Extract text
CHROOT "$PATH pdftotext test.pdf output.txt"

# Merge PDFs
CHROOT "$PATH pdfunite in1.pdf in2.pdf out.pdf"

# Split PDFs
CHROOT "$PATH pdfseparate in.pdf split-%d.pdf"

# Compress PDF
CHROOT "$PATH gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -o out.pdf in.pdf"
```

## Failure Patterns

- **FP-002 (WRONG_PATH):** Tools not in default chroot PATH → use explicit PATH setting
- **FP-004 (NETWORK):** PDF operations local, no network required
- **FP-006 (TIMEOUT):** Large PDFs may require compression before processing

## Fixes

- **FIX-002 (Wrong Path):** Use `PATH=/usr/sbin:/usr/bin:/sbin:/bin` before tool commands
- **FIX-003 (Wrong Syntax):** Use `pdftotext -layout` for layout-preserving extraction
- **FIX-006 (Timeout):** Use `gs -dPDFSETTINGS=/ebook` for compression before processing large PDFs

## Updates

- **2026-11-25:** PDF tools installed and verified in Alpine Linux chroot. All major operations tested successfully. pdftk missing.
- **2026-11-25:** Capability: READY (with pdftk limitation documented).
