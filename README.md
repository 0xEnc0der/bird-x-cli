# Bird CLI v3.0.0 — Free X/Twitter CLI

A fast, free X/Twitter CLI that uses browser cookies (GraphQL) instead of the paid X API. Post tweets, reply, read threads, search — no API key needed.

**Fork of [zaydiscold/bird](https://github.com/zaydiscold/bird) v0.9.0** with critical fixes for June 2026 X API changes.

## What's Fixed in v3.0.0

1. **Transaction ID double-generation bug** — `prepareTransactionId()` was generating a transaction ID before the URL was known, causing a mismatch. Fixed by removing it and letting `fetchWithTimeout()` handle all transaction ID generation from the actual request URL path.

2. **Documented env file corruption issue** — Writing credential env files with bash heredoc/cat corrupts tokens containing special characters. The README now includes Python-based env file writing instructions.

## Requirements

- Node.js >= 22
- pnpm
- X.com account (for browser cookies)

## Installation

```bash
git clone https://github.com/0xEnc0der/bird-x-cli.git
cd bird-x-cli
pnpm install
rm -rf dist && npx tsc
chmod +x dist/cli.js
ln -sf "$(pwd)/dist/cli.js" ~/.local/bin/bird
```

## Authentication

Bird uses X.com browser cookies. You need `auth_token` and `ct0`.

**Get your cookies:**
1. Log into x.com in your browser
2. Open DevTools → Application → Cookies → x.com
3. Copy `auth_token` (40-char hex) and `ct0` values

**Create the env file (IMPORTANT: Use Python, NOT bash):**

```python
import os
env_path = os.path.expanduser('~/.config/bird/env')
os.makedirs(os.path.dirname(env_path), exist_ok=True)

auth_token = "your_40_char_auth_token"
ct0 = "your_ct0_token"

with open(env_path, 'w') as f:
    f.write(f'export AUTH_TOKEN="{auth_token}"\n')
    f.write(f'export CT0="{ct0}"\n')
    f.write('export GUEST_ID="v1%3A..."\n')
    f.write('export LANG="en"\n')
```

**Verify:** `python3 -c "print(len(open(os.path.expanduser('~/.config/bird/env')).read().split('\"')[1]))"` — must be exactly 40.

**⚠️ NEVER use bash heredoc/cat to write the env file** — special characters in tokens get mangled.

## Quick Reference

| Action | Command |
|---|---|
| Post tweet | `bird tweet "Hello world!"` |
| Reply | `bird reply <id-or-url> "Reply text"` |
| Read tweet | `bird read <id-or-url>` |
| Thread | `bird thread <id-or-url>` |
| Search | `bird search "query" -n 10` |
| Mentions | `bird mentions -n 10` |
| Replies to post | `bird replies <id-or-url>` |
| Like | `bird like <id>` |
| Repost | `bird repost <id>` |
| Bookmark | `bird bookmark <id>` |
| Check auth | `bird check` |
| JSON output | Add `--json` to any command |

## Troubleshooting

| Symptom | Fix |
|---|---|
| `HTTP 401` on tweet/reply | Verify auth_token is 40 chars using Python. If wrong, rewrite env with Python. |
| `HTTP 401` on read | Same — verify env file first |
| `HTTP 404` on search | Query ID stale — run `pnpm run graphql:update` and rebuild |
| `HTTP 422` GraphQL validation | Query ID wrong — update `src/lib/query-ids.json` and rebuild |

## Updating Query IDs

X rotates GraphQL query IDs periodically:

```bash
cd bird-x-cli
pnpm run graphql:update
rm -rf dist && npx tsc
```

## Dependencies

- `x-client-transaction-id` (^0.2.0) — Generates `x-client-transaction-id` header
- `commander` (^14.0.2) — CLI framework
- `json5` (^2.2.3) — Config file parsing
- `kleur` (^4.1.5) — Terminal colors

## License

MIT

## Attribution

- Fork: [zaydiscold/bird](https://github.com/zaydiscold/bird) v0.9.0 (June 2026)
- Original: [jawond/bird](https://github.com/jawond/bird) (steipete)
- Transaction ID: `x-client-transaction-id` npm package
- v3.0.0 patches: 0xEnc0der (June 2026)
