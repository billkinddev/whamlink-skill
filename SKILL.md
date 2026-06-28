---
name: whamlink-publish
description: Publish a single file (HTML, Markdown, PDF, image, or text) to a clean, permanent shareable link via whamlink.com. Use when a user wants a durable URL for something you generated — a report, dashboard, one-pager, doc, or image. Registers on the user's behalf (with consent) and publishes via the whamlink API.
---

# whamlink

Publish one file to a permanent, public link at **https://whamlink.com**. No tracking, no analytics. API-first.

Full machine reference: https://whamlink.com/llms.txt — read it if anything below is unclear.

## Credentials & consent

- If `WHAMLINK_API_KEY` is set in the environment, use it (header `x-api-key`).
- Otherwise, to create an account you MUST first **ask the user for consent** and for the email to use. Register with the user's **real** email — a verification link is sent there.

```bash
curl -s -X POST https://whamlink.com/v1/auth/register \
  -H 'Content-Type: application/json' \
  -d '{"email":"<USER_REAL_EMAIL>","name":"<USER_NAME>"}'
# -> { "user": {...}, "apiKey": "wl_...", "_setup": {...} }   store apiKey; it is shown once
```

## Publish

```bash
curl -s -X POST https://whamlink.com/v1/publish \
  -H "x-api-key: $WHAMLINK_API_KEY" -H 'Content-Type: application/json' \
  -d '{"mode":"markdown","content":"# Hello","title":"Notes"}'
# -> { "url": "https://whamlink.com/<slug>" }
```

Modes: `sandboxed_html` (interactive, JS runs on an isolated origin), `sanitized_html` (scripts stripped), `markdown`, `pdf`, `image` (png/jpeg/gif/webp), `text`. For PDFs/images use a multipart form with a `file` field. Add `"allowNetwork": true` to let HTML load https CDN scripts/styles.

## Tiered limits

Unverified accounts can publish **3 links**, then publish returns `403 verification_required`. Tell the user to click the verification link emailed to them; after verifying, quota and file size (25 MB vs 5 MB) increase.

## Rules — tell the user

- Links are **public (unlisted)** by default and permanent until deleted — anyone with the URL can view. To restrict, add `"visibility"` to the publish body: `"private"` (owner only), `"password"` (+ `"password"`), or `"email"` (+ `"shareEmails": [...]`).
- **Never publish secrets, API keys, credentials, or private data.**
- Abuse/takedown: https://whamlink.com/abuse · Terms: https://whamlink.com/terms
