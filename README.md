<div align="center">
  <p><img src="https://avatars.githubusercontent.com/u/187108398?s=200&v=4" alt="VIP Content Bot logo" width="120" height="120" /></p>
  <h1>VIP Content Bot</h1>

  <p>A production Telegram bot for creators — drop exclusive media for fans behind auto-expiring access codes: upload once, share a 5-char code, it vanishes on timeout.</p>
  <p>
    <img alt="Bun" src="https://img.shields.io/badge/Bun-%23000000.svg?style=flat&logo=bun&logoColor=white"/>
    <img alt="Typescript" src="https://img.shields.io/badge/TypeScript-3178C6.svg?style=flat&logo=typescript&logoColor=white"/>
    <img alt="Telegram" src="https://img.shields.io/badge/Telegram-26A5E4.svg?style=flat&logo=telegram&logoColor=white"/>
    <img alt="LMDB" src="https://img.shields.io/badge/LMDB-FF8C00.svg?style=flat"/>
    <img alt="License" src="https://img.shields.io/badge/License-Proprietary-lightgrey.svg"/>
  </p>
</div>

<div align="center">
  <img src="./user-journey.png" alt="VIP Content Bot user journey" width="100%" />
</div>

> **Closed source.** This repo is the public showcase — README, configuration
> template, and license. The full source is licensed separately (see
> [Get the bot](#-get-the-bot)).

## ✨ Showcase

### For fans

- 🎟️ **Redeem by code** — `/start A7KQ2` or a deep link like `https://t.me/<bot>?start=A7KQ2`
- 🖼️ **Media arrives intact** — photos, videos, and mixed albums land exactly as uploaded, under spoiler
- 🚪 **Join gate** — creators can require joining partner chats first; checks are automatic and links are personal one-time invites
- 🌍 **Bilingual** — English + Indonesian messages
- ❌ **Clear failures** — invalid or expired codes get an explicit reply instead of silence

### For creators

- 📤 **Upload anything** — one photo, video, or album → one 5-char code (`A7KQ2`)
- ⏱️ **Auto-expiry** — codes die on your schedule (default 60s); a background worker wipes the evidence messages
- 📊 **Admin panel** — content browser with views and popular charts, searchable user directory with country stats, subscriber and ban management — all inside Telegram, no dashboard to host
- 📣 **Broadcast** — push a message to every subscriber with a sent/failed report
- ⚙️ **Tunable in-chat** — expiry, cleanup cadence, and gate links adjustable without restarts or redeploys

### Platform

- 🔌 **Self-healing polling** — drops reconnect with exponential backoff and no lost updates, so a network blip never needs a manual restart
- 💾 **Embedded database** — no Postgres/Redis to operate; backups are scheduled snapshots with S3-compatible upload
- 🛡️ **Graceful shutdown** — workers drain, timers stop, the database closes cleanly
- 🧪 **E2E tested** — every user journey is asserted end to end before release

## 🤖 Live demo

Try the fan flow right now — no install, just Telegram:

1. Open [SECURITY_DATA](https://t.me/YOUR_DEMO_BOT) _(replace with your demo bot)_
2. Send `/start DEMO1` (or tap the deep link the demo bot shows)
3. Pass the gate if one is configured, receive the media under spoiler

<!-- TODO(owner): point the link above at a real demo bot before publishing. -->

## 💰 Pricing

One-time commercial license per bot instance:

- Full source code + this configuration template
- Setup guidance for polling, webhook, or serverless deploys
- The `.env.example` in this repo documents all 30 options

Final price and payment terms are agreed directly — no storefront cut, no subscription.

## 🚀 Get the bot

VIP Content Bot is commercial, closed-source software:

- 💬 Telegram: [SECURITY_DATA](https://t.me/YOUR_USERNAME) _(replace with your username)_
- 📧 Email: [SECURITY_DATA] _(replace with your email)_

<!-- TODO(owner): put the real contact above before publishing. -->

You receive the full source, the `.env.example` in this repo (all 30 options), and setup guidance for polling, webhook, or serverless deploys.

## 🔧 Configuration preview

Everything is tuned through environment variables (see [`.env.example`](./.env.example) for all options with defaults):

| Variable             | What it does                                  |
| :------------------- | :-------------------------------------------- |
| `TELEGRAM_BOT_TOKEN` | Bot token from [@BotFather](https://t.me/BotFather) (required) |
| `TELEGRAM_BOT_DEPLOY`| `polling` · `webhook:<public-url>` · `serverless:<adapter>` |
| `BOT_ADMIN_IDS`      | JSON array of admin Telegram user IDs (required) |
| `CONTENT_EXPIRY_MS`  | Code lifetime, default 60s                    |
| `BACKUP_ENABLED`     | Scheduled snapshots, off by default           |
| `BACKUP_S3_*`        | Optional upload to R2 / S3 / MinIO            |

Gate chats, invite links, and backups can additionally be managed from inside Telegram itself — no restarts, no redeploys.

## 🧰 Tech Stack

- [Gramstax](https://github.com/gramstax/gramstax) `2.x` — declarative Telegram bot framework
- [@gramstax/lmdb](https://www.npmjs.com/package/@gramstax/lmdb) `2.x` — typed embedded database
- [@gramstax/worker-thread](https://www.npmjs.com/package/@gramstax/worker-thread) `2.x` — worker lifecycle + RPC
- [Bun](https://bun.sh) — runtime, test runner, and bundler
- [@aws-sdk/client-s3](https://www.npmjs.com/package/@aws-sdk/client-s3) `^3` — S3-compatible backup upload
- TypeScript in strict mode

## ❓ FAQ

**Is the source code included?**
No — this repo is the showcase. The full source is licensed separately (see [Get the bot](#-get-the-bot)).

**What do I need to run it?**
[Bun](https://bun.sh), a VPS or any always-on machine, and a bot token from [@BotFather](https://t.me/BotFather). No database server to operate — storage is embedded, backups are built in.

**Polling or webhook?**
Both, plus serverless adapters. Polling fits most creator bots; webhooks fit high-traffic ones. It is one environment variable either way.

**Do fans need to join my channel first?**
Only if you configure the join gate. Promote the bot to admin, tap Add, and it mints personal one-time invite links by itself. No gate configured means codes work instantly.

**What happens when a code expires?**
It stops redeeming and the worker deletes every delivered copy it can reach, then reports leftovers in your admin panel.

**Can I ban someone?**
Yes — per-user bans from the admin panel, enforced silently on every entry point.

**Which languages do fans see?**
English and Indonesian, picked automatically from the fan's Telegram language.

**Are my backups safe?**
Snapshots run on schedule with retention, optionally uploaded to any S3-compatible storage (R2, AWS, MinIO) with verified PUTs. Upload failures never delete the local file.

## 📄 License

Proprietary — see [LICENSE](./LICENSE).
