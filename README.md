# whamlink-publish (Claude skill)

A [Claude](https://claude.com/claude-code) skill that publishes a single file — HTML, Markdown, PDF, image, or text — to a clean, permanent, shareable link via [whamlink.com](https://whamlink.com).

Use it when you want a durable URL for something Claude generated: a report, dashboard, one-pager, doc, or image. Claude registers an account on your behalf (with your consent and real email) and publishes via the whamlink API.

## Install

Copy `SKILL.md` into a skill folder where your Claude client discovers skills, e.g.:

```bash
mkdir -p ~/.claude/skills/whamlink-publish
curl -fsSL https://raw.githubusercontent.com/billkinddev/whamlink-skill/main/SKILL.md \
  -o ~/.claude/skills/whamlink-publish/SKILL.md
```

Then ask Claude to "publish this to whamlink" and it will use the skill.

Optionally set `WHAMLINK_API_KEY` in your environment to skip registration (get a key at <https://whamlink.com/app>).

## What it does

- Registers an account on the user's behalf (with consent + real email), or uses `WHAMLINK_API_KEY`.
- Publishes one file and returns a permanent `https://whamlink.com/<slug>` link.
- Links are **public (unlisted)** by default; set `visibility` to `private`, `password`, or `email` to gate them.
- **Never publishes secrets, API keys, or private data.**

Full machine-readable API reference: <https://whamlink.com/llms.txt>

## License

MIT — see [LICENSE](LICENSE).
