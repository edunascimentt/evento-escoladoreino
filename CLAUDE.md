# CLAUDE.md — evento-escoladoreino

Context for any Claude Code session working on this repo. Talk to the user in Portuguese (pt-BR);
code, comments and commits stay in English.

## What this is

Capture page for the free live class **"O arrebatamento no final dos tempos"** (series Escatologia
Completa, teacher Eduardo Reis, Thu 2026-11-05 20:00 BRT) of the Escola do Reino.

- Live: https://escatologia.escoladoreino.com (also https://evento-escoladoreino.polaris-web.workers.dev)
- Flow: `/` (event page + 3-step form: name → WhatsApp → e-mail + consent) → `/grupo`
  (button to the WhatsApp group; the class link is only sent inside the group).
- One Cloudflare Worker (`evento-escoladoreino`) serving static files from `public/` and a few
  routes in `src/index.ts`.

## Deploy

- **Push to `main` deploys automatically** (Cloudflare Workers Builds connected to this repo).
  No Cloudflare login is needed to ship a change: edit, commit, push, wait ~1 min, check the site.
- Manual deploy (only for someone logged into the Cloudflare account): `npx wrangler deploy`.
- Never deploy over the Worker `escoladoreinoclub` (the main site escoladoreino.com) or
  `escoladoreino` (another project). This repo only owns `evento-escoladoreino`.

## Where things are

| Change | File |
|---|---|
| Texts, sections, form steps of the event page | `public/index.html` |
| Thank-you / WhatsApp group page (team copy) | `public/grupo.html` |
| Look and animations | `public/assets/style.css` |
| Page behaviour (countdown, steps, validation, scroll effects) | `public/assets/app.js` |
| Group page (confetti, button) | `public/assets/grupo.js` |
| Meta Pixel + event ids | `public/assets/pixel.js` |
| Server: sign-up, group redirect, CSV, Conversions API | `src/index.ts` |
| WhatsApp group link, class name/date ids, pixel id | `wrangler.jsonc` → `vars` |
| WhatsApp group cover image | `entregas/icone/icone.html` (render to PNG 2160×2160) |

- **WhatsApp group link:** `WHATSAPP_GROUP_URL` in `wrangler.jsonc`. Change it, push.
- **Class date/time:** appears in `public/index.html` (`data-countdown="2026-11-05T20:00:00-03:00"`,
  twice, plus visible texts "5 de novembro", "Qui · 05/11 · 20h", ticker, sticky bar),
  `public/grupo.html` (recap list) and `AULA_NOME` in `wrangler.jsonc`.

## Data (important)

- Sign-ups are written into the **main site's D1 database** `escoladoreino-club` (shared with
  escoladoreino.com; schema owned by the private repo `edunascimentt/escoladoreinoclub`).
  Each sign-up = a row in `leads` (status `cadastrou`, `landing_path=/aula-arrebatamento`) + an
  `aula_inscricao` event; the group button adds an `aula_grupo` event.
- **Never create, alter or drop tables from this repo.** Never delete leads.
- Leads are visible at https://escoladoreino.com/admin → Leads, and as CSV (this class only) at
  `/admin/inscritos.csv` (HTTP Basic, password = secret `ADMIN_PASSWORD`).

## Secrets (Cloudflare → Worker → Settings → Variables and Secrets; never in the repo)

- `ADMIN_PASSWORD` — CSV export password.
- `META_CAPI_TOKEN` — Meta Conversions API token (pixel 1818933942616750, "Pixel Eduardo Reis").
- Local dev: copy them into `.dev.vars` (gitignored).

## Rules

- This repo is **public**: never commit tokens, passwords, `.dev.vars`, lead data or audio files.
- Keep the visual identity: Tawny #DB5500, Isabelline #EDE7E1, Night #0C0F10, Poppins + Roboto.
  The owner wants rich, animated pages, never plain ones.
- Every animation must stay off under `prefers-reduced-motion`, and the page must work without JS.
- Test on a phone-sized viewport (390px) before pushing: no horizontal scroll.

## Local dev

```bash
npm install
for f in ../escoladoreinoclub/migrations/*.sql; do npx wrangler d1 execute DB --local --file "$f"; done  # needs the private repo
npx wrangler dev        # http://localhost:8787
npx tsc --noEmit        # typecheck
```

Without the private repo, static pages still render with `npx wrangler dev`; only the sign-up
endpoint needs the tables.
