# Telegram CAPTCHA Bot

A Telegram group join-request bot that privately verifies new members with a math CAPTCHA and provides a public health endpoint for uptime monitoring.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — build and run the API, monitor backend, and Telegram poller together.
- `pnpm run build` — typecheck and build workspace packages for publishing.
- `pnpm --filter @workspace/telegram-join-captcha test` — run CAPTCHA, join-request, timeout-recovery, and process-lock tests.
- Required Replit Secrets: `TELEGRAM_BOT_TOKEN` and `ADMIN_CHAT_ID`.
- Public UptimeRobot endpoint: `/telegram-bot-health` (returns HTTP 200 only while Telegram polling is active; otherwise HTTP 503).
- Dashboard status endpoint: `/api/bot-status`.

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Telegram polling bot: `node-telegram-bot-api`
- API and health routes: Express 5
- Public monitor page: React + Vite

## Operations

- The API server owns the single active Telegram polling process. Do not start the standalone bot package while the API workflow or a deployment is polling with the same token.
- The bot uses a local process lock as an additional safeguard. It cannot prevent another Replit project from polling with the same token.
- Keep bot tokens and chat IDs in Replit Secrets; never add them to source files or logs.
- CAPTCHA join approval, timeout cleanup, and appeals are established product behavior; limit hosting changes to infrastructure unless the user asks otherwise.
