# Free-Professional-Email-setup
Step-by-step guide for setting up a professional custom domain email (send &amp; receive) using Zoho Mail, Gmail, and Cloudflare at zero cost.
# Free Professional Email Setup (Zoho + Gmail + Cloudflare)

Set up **send + receive** email on your own domain **for free**, using:
- Zoho Mail (Free plan) for mailbox + SMTP
- Cloudflare Email Routing for inbound forwarding
- Gmail as your day‑to‑day client (send via SMTP, receive via forwarding)

> Works without paying for Google Workspace or Zoho paid plans.

## Diagram
Domain → **Cloudflare (MX)** → forwards to **Gmail**  
Gmail (Compose) → **Zoho SMTP** → recipient

## Prerequisites
- A domain (I used Namecheap)
- A free Cloudflare account
- A free Zoho Mail account (Custom Domain, 1 user)
- A Gmail account

## Step 1 — Add domain to Cloudflare
1. Add `YOURDOMAIN` to Cloudflare.
2. At your registrar, switch nameservers to Cloudflare’s (e.g., `adel.ns.cloudflare.com`, `ricardo.ns.cloudflare.com`).

## Step 2 — Configure MX for Cloudflare Email Routing
1. In Cloudflare: **Email → Email Routing → Get started**.
2. Create a custom address (e.g., `support@YOURDOMAIN`) and set **Destination** to `YOUR_GMAIL`.
3. Let Cloudflare add the **MX** and **TXT** it suggests; confirm nameservers have propagated.
4. Wait for **Routing status: Enabled**.

## Step 3 — Create a mailbox in Zoho
1. Add your domain in Zoho Mail Admin and create the user (e.g., `support@YOURDOMAIN`).
2. (On free plan) **do not** point MX to Zoho; leave Cloudflare’s MX in place.

## Step 4 — Enable Gmail “Send mail as” via Zoho SMTP
Gmail: **Settings → Accounts and Import → Send mail as → Add another email address**

Use:
- SMTP server: `smtp.zoho.com`
- Port: **587** (TLS) or 465 (SSL)
- Username: `support@YOURDOMAIN`
- Password: **ZOHO APP PASSWORD** (recommended)
- Confirm ownership via the code Zoho sends.

## Step 5 — Optional: Gmail fetch (POP/IMAP)
- POP/IMAP from Zoho to Gmail is **paid on Zoho**; we use Cloudflare forwarding instead.
- You can still use Zoho’s webmail directly if you want.

## DNS Reference (what your final DNS should look like)
| Type | Name         | Value / Target                         | Proxy |
|------|--------------|-----------------------------------------|-------|
| MX   | @            | `route1.mx.cloudflare.net` (prio 6)    | DNS only |
| MX   | @            | `route2.mx.cloudflare.net` (prio 73)   | DNS only |
| MX   | @            | `route3.mx.cloudflare.net` (prio 87)   | DNS only |
| TXT  | @            | Cloudflare routing verification         | —     |
| TXT  | @            | `v=spf1 include:zoho.com ~all`         | —     |

> You may also add DKIM for Zoho (Mail Admin → Email Authentication → DKIM) and a DMARC policy later.

## Troubleshooting
- **Mail lands in Spam**
  - Finish SPF/DKIM; add a simple, personal‑looking first email; avoid links/images; warm up gradually.
- **Gmail can’t send via Zoho**
  - Use an **app password**; verify correct SMTP port (587 TLS or 465 SSL).
- **Cloudflare shows “Syncing”**
  - Wait for NS propagation (often up to a few hours); check with `nslookup -type=mx YOURDOMAIN`.

## Notes
- This repo documents the process; no service credentials included.
- Tested: 8/14/2025 / Sacramento, CA

