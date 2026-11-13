# DERVIQ FileVault

**Offline, browser-based file encryption with AES-256-GCM.**

DERVIQ FileVault is a single-file HTML utility. It encrypts and decrypts files locally in your browser using the built-in Web Crypto API. No third-party JavaScript, external stylesheet, analytics, account, or runtime network dependency is required.

## Features

- AES-256-GCM authenticated encryption with a 128-bit authentication tag.
- PBKDF2-HMAC-SHA-256 key derivation with 600,000 iterations.
- Fresh 16-byte salt and 12-byte random IV for every encrypted file.
- Authenticated file header; original filename and MIME type remain encrypted.
- Multi-file queue, drag-and-drop, and local downloads.
- Persian right-to-left interface.

## Run

1. Download [index.html](index.html) or the [direct raw file](https://raw.githubusercontent.com/DERVIQTECH/derviq-filevault/main/index.html).
2. Open it in a recent browser (Chrome, Edge, or Firefox).
3. Choose encryption, select files, enter a unique passphrase of at least 12 characters twice, then encrypt.
4. Keep the `.dvqf` file and passphrase in separate safe places.
5. For recovery, switch to decryption and provide the exact same passphrase.

Web Crypto requires a secure context. HTTPS and local `file://` pages are generally treated as potentially trustworthy by modern browsers, but policies can vary. The app checks Web Crypto availability at startup.

## Resource limits

- Source file: 63 MiB maximum.
- Encrypted `.dvqf` container: 64 MiB maximum.
- Total queue: 128 MiB maximum.
- Up to 100 files per queue.

These limits reduce memory pressure but do not guarantee operation on every device. Data and cryptographic intermediates are buffered in memory.

## Privacy and limitations

The application does not use `fetch`, XMLHttpRequest, WebSocket, `sendBeacon`, remote scripts, or remote stylesheets. Processing is local after the page is loaded. Downloading the HTML from a website still makes an initial request to that website.

The app cannot protect against malware, hostile extensions, keyloggers, compromised browsers/operating systems, or physical access to an unlocked device. JavaScript cannot guarantee secure erasure of all secrets/plaintext from memory. Forgotten passphrases cannot be recovered. Keep independent backups.

This project has had source review and focused cryptographic smoke tests, not a formal independent audit or certification. See [SECURITY-AUDIT.md](SECURITY-AUDIT.md).

## Files

- `index.html` — standalone application.
- `SECURITY.md` — vulnerability reporting policy.
- `SECURITY-AUDIT.md` — review scope, fixes, tests, limitations.
- `CHANGELOG.md` — release history.

## References

- [MDN Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
- [MDN SubtleCrypto.deriveKey](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/deriveKey)
- [MDN AES-GCM parameters](https://developer.mozilla.org/en-US/docs/Web/API/AesGcmParams)
- [MDN Secure Contexts](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Secure_Contexts)
- [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final)

---

DERVIQ Technologies · FileVault 1.0.1
