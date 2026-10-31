# Evento Escola do Reino — aula "O arrebatamento no final dos tempos"

Sign-up page for the free live class (Escatologia Completa, Eduardo Reis, Wed 2026-11-04 20:00 BRT).
Flow: `/` (capture form) → `/grupo` (button to the WhatsApp group, where the class link is sent).

- Worker `evento-escoladoreino` — https://escatologia.escoladoreino.com (also https://evento-escoladoreino.polaris-web.workers.dev). Push to `main` deploys. See CLAUDE.md for the full guide.
- Writes into the **same D1 as escoladoreino.com** (`escoladoreino-club`): each sign-up is a `leads`
  row (status `cadastrou`, `landing_path=/aula-arrebatamento`) + an `aula_inscricao` event; clicks on
  the group button add an `aula_grupo` event. They show up in escoladoreino.com/admin → Leads.
  Schema is owned by `../escoladoreinoclub/migrations` — this project never alters tables.
- Same person (WhatsApp or e-mail) already in `leads` is updated, never duplicated.
- CSV of this class only: `/admin/inscritos.csv` (HTTP Basic, any user, password = secret `ADMIN_PASSWORD`).
- Meta Pixel (same as the site): PageView + Lead, browser only.

## WhatsApp group link

Set `WHATSAPP_GROUP_URL` in `wrangler.jsonc` (must start with `https://chat.whatsapp.com/`) and run
`npm run deploy`. Until then `/grupo` shows "convite sendo liberado" and re-checks every 30 s.

## Custom subdomain

Add to `wrangler.jsonc`: `"routes": [{ "pattern": "aula.escoladoreino.com", "custom_domain": true }]`,
set `"workers_dev": false`, deploy.

## Local

```bash
npm install
for f in ../escoladoreinoclub/migrations/*.sql; do npx wrangler d1 execute DB --local --file "$f"; done
npx wrangler dev   # .dev.vars: ADMIN_PASSWORD=...
```
