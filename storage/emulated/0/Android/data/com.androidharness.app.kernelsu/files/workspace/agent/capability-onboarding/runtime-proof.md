# Runtime Proof — Capability Attestation

Generated: 2026-11-25

## Verification Commands and Results

| Command | Exit Code | Output | Status | Failure Reason |
|---------|-----------|--------|--------|----------------|
| `python3 --version` | 0 | Python 3.14.10 | READY | - |
| `pip3 --version` | 0 | pip 26.3 from /data/user/0/com.androidharness.app.kernelsu/files/linux/lib/python3.14/site-packages/pip (python 3.14) | READY | - |
| `bash --version` | 0 | GNU bash, version 5.3.20(1)-release (aarch64-unknown-linux-android) | READY | - |
| `git --version` | 0 | git version 2.56.1 | READY | - |
| `curl --version` | 0 | curl 8.23.0 (aarch64-unknown-linux-android) libcurl/8.23.0 OpenSSL/3.6.5 zlib/1.4 libssh2/1.11.1 nghttp2/1.71.0 ngtcp2/1.26.0 nghttp3/1.19.0 Release-Date: 2026-10-14 Protocols: dict file ftp ftps gopher gophers http https imap imaps ipfs ipns mqtt mqtts pop3 pop3s rtsp scp sftp smtp smtps telnet tftp ws wss Features: alt-svc AsynchDNS HSTS HTTP2 HTTP3 HTTPS-proxy IPv6 Largefile libz SSL threadsafe UnixSockets | READY | - |
| `node --version` | 0 | v26.13.1 | READY | - |
| `npm --version` | 0 | 12.2.3 | READY | - |
| `rustc --version` | 0 | rustc 1.100.0 (0ef1bc386 2026-11-09) (built from a source tarball) | READY | - |
| `gcc --version` | 0 | clang version 21.1.8 Target: aarch64-unknown-linux-android24 Thread model: posix InstalledDir: /apex/com.android.runtime/bin | READY | - |
| `java -version` | 127 | /data/user/0/com.androidharness.app.kernelsu/files/linux/bin/bash: line 1: java: command not found | NOT VERIFIED | `java: command not found` (exit 127) |

## Summary

- **Total commands attempted:** 10
- **Commands passed (READY):** 9
- **Commands failed (NOT VERIFIED):** 1
- **Failed command:** `java -version`

## Issue Classification

- **Command length:** NOT the issue — commands were single and simple.
- **Quoting:** NOT the issue — no nested quotes or weird syntax.
- **Shell environment:** The java failure is due to missing binary, not shell syntax.
- **Environment issue:** `java` is not installed in this environment (confirmed by capabilities note: `NOT_AVAILABLE: java, javac, gradle, cmake, make`).

## Next Steps

1. Install Java if needed for future builds (optional).
2. Continue capability attestation for remaining tools as they become relevant.
