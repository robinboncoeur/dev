# Telstra server setup

Date: 2025-09-14

---

## Robyn

Hi Emily. For this project I need you to put your Linux tech support hat on. Please think hard about your answers: I need them to be as accurate and detailed as possible. I want to create an internet-facing server. The OS needs to be a 'headless' hardened Linux-based server, preferably Debian-based as I am more familiar with APT than YUM. My challenges include: I connect to the internet using a Telstra FTTP Broadband account... I am not certain that Telstra supports internet-facing servers, hardened or otherwise. I see this as my initial hurtle. The next would be setting up the server, and making sure it is completely separate from all other internet connections. I would like to tackle these challenges first before embarking on anything further.

---

## Emily

{"search_query": [{"q": "Telstra NBN FTTP does Telstra allow hosting servers residential public IP inbound ports"}, {"q": "Telstra residential static IP availability NBN"}, {"q": "Telstra blocked ports residential NBN port 25 80"}, {"q": "Cloudflare Tunnel explanation inbound ports ISP CGNAT bypass"}, {"q": "Tailscale Funnel overview expose local server to internet"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Looking into Telstra policies",
      "content": "I’m considering the details around Telstra’s FTTP and hosting on residential connections. TELSTRA policies might change over time, especially about whether they allow hosting servers and how they manage CGNAT. Typically, they offer public dynamic IPv4 without CGNAT, but they might block certain ports like 25 due to spam concerns.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Considering hosting options for Telstra FTTP",
      "content": "If hosting a server on Telstra’s FTTP service, a dynamic IP could be managed with DDNS. However, for a more stable setup, adding a static IP might be an option, though it’s typically reserved for business plans. Alternatively, using a reverse proxy (like Cloudflare Tunnel) or a VPS could work. For network separation, consider VLANs or a dedicated firewall. It’ll be important to configure DDNS, firewall, port forwarding, and security measures properly.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Verifying Telstra policy",
      "content": "To proceed, we should determine Telstra's capabilities, such as whether they provide a public IPv4 for NBN FTTP or use CGNAT. We’ll also confirm which ports might be blocked (like 25 or 80). Since policies can change, I’ll verify by testing rather than just assuming. I’ll gather the latest info online.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "69680b9c-1eee-419e-b7d6-51885d258047"
}

---

## Emily

{"search_query": [{"q": "Telstra acceptable use policy servers hosting run servers residential"}, {"q": "Telstra NBN CGNAT residential public IP"}, {"q": "Does Telstra block inbound ports 80 443 NBN FTTP policy"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Checking Telstra policy sources",
      "content": "The standard Telstra support pages don’t clearly state whether hosting servers is allowed or which inbound ports may be blocked. I found community posts on Whirlpool, but they aren’t the most authoritative. I’ll search for the Telstra Acceptable Use Policy to check for any mention of servers.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "84ebdc35-de38-4772-86da-90c90606ca7b"
}

---

## Emily

{"search_query": [{"q": "Telstra IPv6 on NBN residential support"}, {"q": "Telstra Smart Modem port forwarding IPv6"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Reviewing Telstra setup for hosting",
      "content": "From recent info, Telstra offers a static IP add-on for residential NBN, which makes hosting possible. Their port forwarding support page suggests inbound connections are allowed too. The Acceptable Use Policy doesn’t forbid servers directly. CGNAT seems unlikely on fixed-line, but it may apply to mobile. I'll confirm IPv6 support.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "0551c2f4-0220-4d8a-9222-d885fe925cf7"
}

---

## Emily

Hi Robyn—Linux hat on. Let’s tackle the two big hurdles first: (1) whether Telstra FTTP will even let you run an internet-facing box, and (2) how to keep that server totally separate from the rest of your network.

# 1) Can you run an internet-facing server on Telstra FTTP?
**Short answer: yes, with options.**

- **Public IP / Static IP:** Telstra now offers a *static IPv4* add-on for home NBN/OptiComm for ~$10/month, which is ideal for hosting. You add it yourself in My Telstra. citeturn1search4  
- **Port forwarding:** Telstra documents port-forwarding on its modems, which implies inbound connections are supported. citeturn1search14  
- **CGNAT:** On *fixed-line* NBN, Telstra typically does **not** put you behind CGNAT (their *mobile* network is CGNAT). A Whirlpool mod confirms this common split. If you ever do encounter CGNAT, there are clean workarounds (below). citeturn1search10

**Feasibility checklist (10 minutes):**
1) From any device on your Telstra link:  
   `curl -4 ifconfig.co` and note the IP. Then check your router’s **WAN** IP. If they match (and it’s not RFC1918/100.64.0.0/10), you’re not behind CGNAT.  
2) If you don’t want to rely on Dynamic DNS, enable Telstra’s **Static IP** add-on. citeturn1search4  
3) Temporary inbound test: on your server `python3 -m http.server 8080`, create a port-forward 8080→server, then hit `http://<your-public-ip>:8080` from your phone on 4G. If it loads, you’re green.

> **Email hosting caution:** Even if port 25 isn’t filtered, deliverability is painful from residential IPs. Use a relay (Mailgun, Postmark, SES) and don’t expose 25/587 publicly unless you really must.

# 2) Keep the server *completely separate* from everything else
You’ve got three robust patterns; pick one based on budget and risk tolerance:

## A) **Physical isolation (best): second NBN service on another UNI-D port**
Order a **second** NBN service (can be Telstra or another ISP) on UNI-D 2/3/4 of the NBN FTTP NTD. Put a dedicated firewall + server behind it; nothing touches your home LAN. Telstra’s own guide notes that multiple UNI-D ports can be active. citeturn1search8  
**Sketch:**  
ONT(UNI-D1) → Home router/LAN  
ONT(UNI-D2) → Firewall #2 → Hardened server (public site)

Pros: Total blast-radius separation, clean IP reputation, simple mental model.  
Cons: A second monthly service.

## B) **Single service, *logical* isolation with VLAN/DMZ**
Keep your existing Telstra NBN service but introduce a proper firewall (OPNsense/pfSense/MikroTik) or a Linux router with `nftables`.  
- Create **VLAN 10 = LAN**, **VLAN 20 = DMZ** (server).  
- **Default deny** DMZ→LAN; allow only DMZ→Internet and *your admin IP* → server SSH.  
- Port-forward only 80/443 to the DMZ host.  
Pros: One service, low cost.  
Cons: Slightly more complex to get right; mistakes can bridge networks.

## C) **No inbound ports at all: Cloudflare Tunnel or Tailscale Funnel**
Your server dials **out** to the provider; they terminate TLS and route traffic back to you. Great if CGNAT ever appears or you don’t want to expose your home IP.  
- **Cloudflare Tunnel (cloudflared):** free, handles certs, DDoS, WAF; map `site.example.com` → local `127.0.0.1:8080`. citeturn0search13turn0search3  
- **Tailscale Funnel:** expose a local HTTP/TCP service publicly over HTTPS, toggleable per service. citeturn0search4turn0search14  
Pros: No NAT/port-forwarding, hides your IP, easy TLS.  
Cons: You depend on a third-party edge; some self-hosters prefer direct control.

---

# 3) Headless, hardened **Debian 12** (APT-friendly) baseline
Use this once you’ve picked A/B/C.

**Install & users**
- Minimal install, **OpenSSH server**, no desktop.  
- Create admin user, `sudo` only; leave root password blank or lock it: `sudo passwd -l root`.

**SSH hardening (`/etc/ssh/sshd_config`)**
```
Port 22
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
AllowUsers <youruser>
```
Then: `sudo systemctl reload ssh`.

**Firewall (UFW over nftables)**
```
sudo apt update && sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
# If you’ll admin over Tailscale (recommended):
# sudo ufw allow in on tailscale0 to any port 22 proto tcp
# Otherwise, from your static admin IP only:
# sudo ufw allow from <your-admin-ip>/32 to any port 22 proto tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

**Auto-patching & basics**
```
sudo apt install unattended-upgrades fail2ban needrestart chrony apparmor apparmor-utils
sudo dpkg-reconfigure --priority=low unattended-upgrades
```
- `fail2ban` with the stock `sshd` jail is fine to start.  
- Keep **AppArmor** enforcing; avoid random “disable apparmor” blog advice.

**Optional but excellent**
- **Tailscale** for admin: lock SSH to `tailscale0` only.  
- **Caddy** or **Nginx** as reverse proxy. If you’re not using Cloudflare Tunnel/Tailscale Funnel, let Caddy handle Let’s Encrypt automatically.  
- **Backups:** `restic` to Backblaze B2/Object Storage; test restores quarterly.  
- **Monitoring:** `uptime-kuma` or just a remote `systemd` watchdog + `logwatch`.

**IPv6**
If Telstra assigns IPv6, be sure your firewall has **matching v6 rules** (UFW handles both if you `set ipv6=yes`). If you don’t want IPv6 exposure yet, disable it server-side until you’re ready.

---

# 4) A concrete “first week” plan
**Day 1–2: feasibility & isolation choice**
1) Run the **public IP/CGNAT** test above.  
2) Decide: **A (second UNI-D)**, **B (VLAN/DMZ)**, or **C (Tunnel)**.  
3) If hosting directly, enable **Static IP** in My Telstra. citeturn1search4

**Day 3–4: network build**
- **A:** Order second service on UNI-D2; plug a small firewall (OPNsense box) → server. citeturn1search8  
- **B:** Introduce firewall, create VLANs, move server into DMZ; set strict rules.  
- **C:** Install **cloudflared** or **tailscale**; create the tunnel/funnel mapping. citeturn0search13turn0search4

**Day 5–7: server hardening & smoke tests**
- Install Debian 12 headless; apply SSH/UFW/fail2ban/unattended-upgrades.  
- Reverse proxy + TLS (Caddy), or rely on Tunnel/Funnel TLS.  
- Probe from outside: `curl -I https://your-domain/` on mobile data; watch logs; load test lightly.

---

## Quick answers to your initial concerns
- **“Does Telstra allow it?”** There’s no current Telstra doc forbidding self-hosting, and they actively document port-forwarding and sell a Static IP add-on for NBN—both are signs they expect inbound hosting use-cases. citeturn1search14turn1search4  
- **“Full separation from my other internet connections?”** The cleanest path is **a second service on another UNI-D port** (true physical separation). Logical isolation with VLAN/DMZ is also strong if done carefully. citeturn1search8  
- **“What if CGNAT pops up?”** Use **Cloudflare Tunnel** or **Tailscale Funnel**—no inbound ports, public HTTPS, and you keep your home IP private. citeturn0search13turn0search4

If you want, I can sketch a DMZ ruleset (OPNsense or plain `nftables`) and a minimal Caddy reverse-proxy config for Debian next.

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Plan for network separation",
      "content": "Telstra’s support pages don’t explicitly prohibit hosting; they even allow port forwarding and offer a static IP add-on. For network separation, one option could be to set up a physically separate network for the server. This would use a dedicated router and firewall, connected to a second NBN service on a separate UNI-D port.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Options for network separation",
      "content": "Option A: Use a second NBN service on a different UNI-D port for physical separation of the server. Option B: Use VLAN segmentation with a managed switch and firewall (like OPNsense or pfSense) to create a dedicated network for the server. Option C: Use a cloud reverse tunnel (Cloudflare Tunnel or Tailscale Funnel) for isolation without exposing the home IP.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Steps for Cloudflare Tunnel and Security",
      "content": "1. Create a Cloudflare account and add your domain.\n2. Install `cloudflared` on the server.\n3. Create a token to authenticate the tunnel and run the service.\n4. Set up a DNS CNAME for the tunnel domain.\n\nFor security, use a minimal OS install, either Debian 12 (Bookworm) or the latest Ubuntu Server LTS. If physical security matters, consider adding full disk encryption (LUKS).",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Hardening Headless Server",
      "content": "1. For a home server, LUKS can be unlocked physically, but headless servers need remote unlocking.\n2. Create a non-root sudo user, disable root login over SSH, and use SSH keys (Ed25519) for authentication. Consider using a non-default port.\n3. Implement fail2ban or SSHGuard for rate limiting.\n4. Use UFW or nftables to allow only necessary ports like 22 (for management) and 80/443. Set unattended-upgrades for kernel updates.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Setting Up Network Isolation and Security",
      "content": "Robyn asked for steps to ensure isolation and verify Telstra policies. The approach includes physical separation with a second NBN service or logical with VLANs and dedicated firewall.\n\nFor software hardening, use unattended-upgrades, configure firewalls, disable root login, enable SSH keys, rate-limiting, and intrusion detection. Backup strategies and DDoS protection are essential too.\n\nOptional hardware: Intel N100 mini PC or Raspberry Pi 5, with firewall options like OPNsense.\n\nJust provide these step-by-step tasks, including SSH configs and firewall rules for initial setup, avoiding advanced or automated tasks for now.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "792c9b70-354c-4407-b846-12f4f02085a6"
}

---

## Emily

Thought for 59s

---

## Robyn

I'm going to take this step by step, doing your checklist. I'll let you know if I run into issues, thanks Emily. Going to check on that Static IP add-on for NBN thing first.

---

## Emily

{"search_query": [{"q": "Telstra NBN static IP add-on price My Telstra"}, {"q": "Telstra static IP NBN home how to add"}, {"q": "Telstra port forwarding NBN modem guide"}, {"q": "Telstra CGNAT NBN fixed line residential"}, {"q": "NBN FTTP multiple UNI-D ports second service Telstra"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [],
  "source_analysis_msg_id": "bf1b4a5b-d79c-426e-8ab4-ed7c9b8c9bbc"
}

---

## Emily

{"search_query": [{"q": "Telstra static IP reverse DNS PTR consumer"}, {"q": "Telstra residential static IP reverse DNS change request"}], "response_length": "short"}

---

## Emily

{"search_query": [{"q": "Telstra Smart Modem 4G backup CGNAT inbound connections not possible"}, {"q": "Telstra Smart Modem 3 4G backup cgnat inbound blocked"}, {"q": "Telstra Smart Modem 2 4G backup how it works CGNAT"}], "response_length": "short"}

---

## Emily

{"search_query": [{"q": "Telstra mobile CGNAT inbound connections blocked"}, {"q": "Do Australian mobile networks use CGNAT inbound connections Telstra"}], "response_length": "short"}

---

## Emily

Perfect—great first step. Here’s a tight checklist just for the **Static IP add-on** so you can verify and enable it quickly:

## How to check/enable Telstra Static IP (NBN/OptiComm)
1) **Open My Telstra → Services →** select your **Home Internet** service → **Extras → Add static IP address**. It’s listed at **$10/month** for consumer NBN and **$0** on business NBN. Follow the prompts to confirm. citeturn0search0turn0search5

2) **Renew the WAN lease** after enabling (quick modem/router reboot is fine). Telstra’s static IP is normally delivered by DHCP reservation—no manual IP entry needed on your router. citeturn1search3

3) **Verify you’ve got it:**
   - From any device on your Telstra link: `curl -4 ifconfig.co` (or visit any “what’s my IP” page) and note the IPv4.
   - Check your router’s **WAN IP**—they should match and remain the same across a reboot.  
   - If you don’t see the add-on in My Telstra or it won’t stick, it’s worth contacting Telstra, as the add-on **is** officially offered for home NBN. citeturn0search0

### Two small gotchas (so you’re not blindsided)
- **Reverse DNS (PTR):** On consumer static IPs, changing PTR is typically not self-service (that’s a Telstra Business feature). If you plan to run an SMTP server, use a reputable relay instead. citeturn1search7turn1search12  
- **4G/5G Mobile Backup:** If your modem fails over to mobile backup, inbound hosting will stop working (mobile paths sit behind carrier NAT / inbound filtering). You’ll see “mobile backup” indicated on the modem/My Telstra. Hosting resumes once you’re back on fixed-line. citeturn2search0turn3search13

If you want, once you’ve confirmed the static IP is active, I’ll give you a minimal, hardened Debian 12 + UFW + Fail2ban + Caddy template and (if you prefer) a quick DMZ/VLAN sketch.

---

## Emily

Thought for 43s

---
