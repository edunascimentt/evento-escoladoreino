# evento-escoladoreino (project memory)

- 2026-10-31: Built for the Escola do Reino team: capture page (not a landing page) for the free live
  class "O arrebatamento no final dos tempos" (Escatologia Completa, Eduardo Reis, 2026-11-04 20h).
  Page 1 = event info + form; page 2 = only the WhatsApp group button (class link is sent only in the group).
- User decisions: separate Worker; workers.dev address for now, Escola do Reino subdomain added later;
  data must land in the existing escoladoreino.com D1/admin (not a separate DB); reuse the site's
  identity and photos from ../escoladoreinoclub.
- Pending: WhatsApp group link (WHATSAPP_GROUP_URL), custom subdomain.
- 2026-10-31: User asked for the sign-up form as one question per step (button opens it, name → WhatsApp → e-mail + consent, 'Continuar' between), inside the same card. Keep capture forms stepped by default for this client.
- 2026-10-31: escoladoreino.com public DNS is at Hostinger (NS dns-parking.com) even though the Cloudflare zone shows active; any new subdomain needs a CNAME at Hostinger pointing to escoladoreino.com (or switch NS to Cloudflare: armfazh/arya.ns.cloudflare.com). Subdomain: escatologia.escoladoreino.com.
- 2026-10-31: Meta Pixel 1818933942616750 (Escola do Reino) on both pages via public/assets/pixel.js; events PageView, ViewContent, IniciouInscricao, InscricaoEtapa, Lead, CompleteRegistration (group click). CAPI live: META_CAPI_TOKEN secret set 2026-10-31 (token validated, events_received=1). Pixel 1818933942616750 is "Pixel Eduardo Reis", same as main site.
- 2026-10-31: Public repo github.com/edunascimentt/evento-escoladoreino (main). CLAUDE.md written as handoff guide for the owner's boss. Workers Builds link must be done in the Cloudflare dashboard (wrangler OAuth token lacks Builds scope; Chrome extension not connected).
