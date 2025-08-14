# Free Professional Email Setup (Zoho + Gmail + Cloudflare)

**🔗 [Live Demo / GitHub Pages Site](https://npc-ken.github.io/Free-Professional-Email-setup/)**

Set up **send + receive** email on your own domain **for free**, using:
- Zoho Mail (Free plan) for mailbox + SMTP
- Cloudflare Email Routing for inbound forwarding
- Gmail as your day-to-day client (send via SMTP, receive via forwarding)

> Works without paying for Google Workspace or Zoho paid plans.

---

## 📚 Table of Contents
1. [How It Works](#-how-it-works)
2. [Prerequisites](#-prerequisites)
3. [Step 1 — Add domain to Cloudflare](#-step-1--add-domain-to-cloudflare)
4. [Step 2 — Configure MX for Cloudflare Email Routing](#-step-2--configure-mx-for-cloudflare-email-routing)
5. [Step 3 — Create a mailbox in Zoho](#-step-3--create-a-mailbox-in-zoho)
6. [Step 4 — Send mail via Zoho SMTP in Gmail](#-step-4--send-mail-via-zoho-smtp-in-gmail)
7. [DNS Reference](#-dns-reference)
8. [Troubleshooting](#-troubleshooting)
9. [Notes](#-notes)

---

## 📌 How It Works
**Inbound:**  
Domain → **Cloudflare (MX)** → forwards to **Gmail**  

**Outbound:**  
Gmail (Compose) → **Zoho SMTP** → recipient

---

## ✅ Prerequisites
- A domain (e.g., Namecheap)
- A free Cloudflare account
- A free Zoho Mail account (Custom Domain, 1 user)
- A Gmail account

---

## 🛠 Step 1 — Add domain to Cloudflare
1. Add `YOURDOMAIN` to Cloudflare.
2. At your registrar, switch nameservers to Cloudflare’s  
   *(e.g., `adel.ns.cloudflare.com`, `ricardo.ns.cloudflare.com`)*.

---

## 🛠 Step 2 — Configure MX for Cloudflare Email Routing
1. In Cloudflare: **Email → Email Routing → Get started**.
2. Create a custom address (e.g., `support@YOURDOMAIN`) → set **Destination** to your Gmail.
3. Let Cloudflare add the **MX** and **TXT** it suggests.
4. Wait until **Routing status: Enabled**.

---

## 🛠 Step 3 — Create a mailbox in Zoho
1. Add your domain in Zoho Mail Admin and create the user (e.g., `support@YOURDOMAIN`).
2. (On free plan) **Do not** point MX to Zoho — leave Cloudflare MX as-is.

---

## 🛠 Step 4 — Send mail via Zoho SMTP in Gmail
**Gmail → Settings → Accounts and Import → Send mail as → Add another email address**  
Use:
- **SMTP server:** `smtp.zoho.com`
- **Port:** 587 (TLS) or 465 (SSL)
- **Username:** `support@YOURDOMAIN`
- **Password:** Zoho App Password (recommended)

Verify via code Zoho sends.

---

## 📜 DNS Reference
| Type | Name | Value / Target | Proxy |
|------|------|----------------|-------|
| MX   | @    | `route1.mx.cloudflare.net` (prio 6)  | DNS only |
| MX   | @    | `route2.mx.cloudflare.net` (prio 73) | DNS only |
| MX   | @    | `route3.mx.cloudflare.net` (prio 87) | DNS only |
| TXT  | @    | Cloudflare routing verification     | — |
| TXT  | @    | `v=spf1 include:zoho.com ~all`      | — |

> Also set up **DKIM** in Zoho for better deliverability.

---

## 🔧 Troubleshooting
- **Spam folder issues:** Warm up email, complete SPF/DKIM, avoid too many links/images.
- **SMTP errors:** Double-check app password and SMTP port.
- **Cloudflare stuck “Syncing”:** Wait for propagation; check with:  
nslookup -type=mx YOURDOMAIN

---

## 📅 Notes
- Process tested **Aug 14, 2025** in Sacramento, CA.
- This repo contains **instructions only**, no credentials.
