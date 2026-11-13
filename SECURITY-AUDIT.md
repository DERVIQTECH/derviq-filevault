# DERVIQ FileVault 1.0.1 — Security Review

**Review date:** 2026-11-13  
**Reviewed artifact:** `index.html`  
**Review type:** source inspection and focused cryptographic smoke tests; not a formal independent audit.

## Summary

PBKDF2-HMAC-SHA-256 derives a non-extractable 256-bit AES-GCM key. Each file uses a fresh random 16-byte salt and 12-byte IV; AES-GCM uses a 128-bit tag and authenticates the fixed header as additional authenticated data. Filename and MIME metadata are inside the encrypted payload. No remote script/style, analytics, or application-level network request was found.

No critical or high-severity cryptographic flaw was identified in this focused review. This is not certification or proof that no vulnerabilities exist.

## Findings and changes

- **Fixed — misleading size message:** the previous rejection threshold and displayed message disagreed; the message and limits now align.
- **Fixed — excessive aggregate memory pressure:** the earlier queue could select many large files. Version 1.0.1 limits source files to 63 MiB, encrypted containers to 64 MiB, aggregate queue size to 128 MiB, and queue count to 100.
- **Strengthened — passphrase threshold:** minimum length increased from 8 to 12 characters. Length alone does not guarantee entropy.

## Controls verified by source inspection

- AES key length: 256 bits; GCM tag: 128 bits.
- IV: 12 random bytes from `crypto.getRandomValues()` per file.
- Salt: 16 random bytes per file.
- PBKDF2 hash: SHA-256; encryption iteration count: 600,000.
- Decryption iteration bounds: 100,000 through 1,000,000.
- Complete fixed header authenticated as AES-GCM additional authenticated data.
- Wrong passphrase or modified ciphertext fails authentication.
- Original names and MIME metadata encrypted; output filenames are random identifiers ending in `.dvqf`.
- User-controlled labels use `textContent` in relevant UI paths; filenames are sanitized before downloads.
- No `fetch`, XMLHttpRequest, WebSocket, `sendBeacon`, remote script/style resources, or localStorage/sessionStorage persistence found.

## Tests performed

JavaScript passed `node --check`. Extracted `deriveKey`, `makeHeader`, `encryptFile`, and `decryptFile` functions were exercised using Node.js Web Crypto:

- Encrypt/decrypt byte-for-byte round trip: **PASS**.
- Restore original filename from authenticated metadata: **PASS**.
- Wrong passphrase rejected: **PASS**.
- One-bit ciphertext modification rejected: **PASS**.
- Fresh salt and IV for repeated encryption: **PASS**.

A full graphical browser automation run was blocked by the environment's local-file/localhost navigation policy. Thus UI rendering and end-to-end file-picker/download behavior were not certified. Manual testing in current Chrome, Edge, and Firefox is recommended before relying on the tool for important files.

## Residual limitations

- Files and cryptographic intermediates are buffered in memory; caps reduce but do not eliminate memory pressure.
- JavaScript cannot reliably erase immutable strings or every plaintext copy from process memory.
- Malicious extensions, compromised browsers/OS, keyloggers, or hostile page environments may access data.
- PBKDF2 is used because it is available in Web Crypto; no claim is made that it is the strongest KDF for every threat model. NIST SP 800-132 is relevant and marked for revision.
- Not externally audited, penetration-tested, or certified; do not use as the sole backup of important files.

## References

- https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API
- https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/deriveKey
- https://developer.mozilla.org/en-US/docs/Web/API/AesGcmParams
- https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Secure_Contexts
- https://csrc.nist.gov/pubs/sp/800/132/final
