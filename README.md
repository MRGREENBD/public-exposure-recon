# Public Exposure Recon

Ethical GitHub/public exposure discovery patterns for authorized security review and internal recon workflows.

This project is intended for legitimate use only:
- your own repositories
- repositories you are explicitly authorized to review
- internal security testing on systems you own or manage

Do not use this against third-party systems without permission.

## Purpose

This repository provides:
- a collection of regex patterns for public exposure review
- Burp Suite search patterns
- a broad, responsible workflow for identifying possible secret exposure paths
- a safe guide for validating findings before escalation

This tool is designed for controlled security review, not unauthorized access or credential theft.

## Contents

- `regexes/secret-patterns.txt` — general secret detection patterns
- `regexes/js-only-patterns.txt` — frontend/JavaScript-focused patterns
- `regexes/burp-one-liner.txt` — single-line Burp Suite search regex
- `README.md` — usage guidance

## Files

### `regexes/secret-patterns.txt`

Contains common patterns for:
- GitHub tokens
- AWS keys
- Google API keys
- Stripe keys
- JWTs
- private keys
- bearer tokens
- config-like secrets

### `regexes/js-only-patterns.txt`

Contains JavaScript-focused patterns for:
- `apiKey`, `secretKey`, `token`, `Authorization`
- frontend config values
- client-side secret-like fields
- fetch/axios/XHR style usage

### `regexes/burp-one-liner.txt`

A single-line Burp Suite regex designed to catch several common secret-like values in one pass.

## How to use in Burp Suite

1. Open Burp Suite.
2. Go to the Proxy tab.
3. Intercept traffic or search through responses.
4. Use the search box or custom scanner regex search.
5. Paste any regex from the files above.

Example:

```text
(?i)(gh[pousr]_[A-Za-z0-9]{20,250}|AKIA[0-9A-Z]{16}|AIza[0-9A-Za-z-_]{35}|xox[baprs]-[A-Za-z0-9-]+|sk_(live|test)_[A-Za-z0-9]+|pk_(live|test)_[A-Za-z0-9]+|sk-[A-Za-z0-9]{20,200}|eyJ[A-Za-z0-9_-]*\.[A-Za-z0-9_-]*\.[A-Za-z0-9_-]*|-----BEGIN (RSA|OPENSSH|PGP|EC) PRIVATE KEY-----|Bearer\s+[A-Za-z0-9\-._~+/]+=*|(?:(?:aws_access_key_id|aws_secret_access_key|access_key_id|secret_access_key|client_secret|private_key|api[_-]?key|secret|token|password|passwd|pwd)[\s'":=]+['"]?[A-Za-z0-9!@#$%^&*()_+=\-\./:;]{8,})|https?://[^:/\s]+:[^@/\s]+@)
```

## GitHub search workflow

Use GitHub search with narrow scope and explicit targets.

Examples:

```text
"API_KEY" repo:your-org/your-repo
"BEGIN PRIVATE KEY" repo:your-org/your-repo
"aws_access_key_id" repo:your-org/your-repo
"secret_key" filename:.env repo:your-org/your-repo
"client_secret" path:config repo:your-org/your-repo
```

Use filters to reduce noise:
- `repo:owner/repo`
- `path:config`
- `filename:.env`
- `extension:js`
- `NOT example`
- `NOT vendor`

## Safe validation process

When a match is found:

1. Confirm it exists in an in-scope repository or environment.
2. Check whether it is a real secret or just a sample/test value.
3. Determine whether it is active or expired.
4. Validate impact only within approved testing scope.
5. Report responsibly through the correct internal or disclosure channel.

## Important policy

This repo is not for:
- unauthorized access
- secret theft
- third-party exploitation
- credential abuse
- bypassing access control

Use only for:
- your own systems
- explicitly authorized targets
- internal security review
- lawful disclosure and remediation

## Suggested workflow

- search public repos under your scope
- review config, env, CI, and deployment files
- look for hardcoded values in JS and config files
- validate before escalation
- redact or avoid storing sensitive matches in reports

## License

MIT
