# Security Policy

## Supported release

Please use the latest published DERVIQ FileVault release and report vulnerabilities privately to the project maintainer through a private contact channel associated with the DERVIQTECH GitHub account.

## Reporting a vulnerability

Do not publish working exploit details for issues that could expose files or passphrases. Include:

- A concise description and security impact.
- Affected release and browser/OS versions.
- Reproduction steps or a minimal proof of concept using non-sensitive test files.
- Suggested mitigation, if known.

Never send real confidential files or passphrases in a report.

## Scope

The security boundary includes the HTML application, file-format parser, password-based key derivation, AES-GCM handling, filename restoration, and browser interactions. The application cannot defend against compromised browsers/operating systems, malicious extensions, keyloggers, or physical access to an unlocked device.
