# 🏠 Emanuel's Homelab — Portfolio Hub

> Electronics/Communications Engineer documenting a production-grade home infrastructure lab: virtualization, media services, automation, monitoring and networking.

This repository is the **index** for a set of small, focused repos — each one documents a self-contained piece of the lab (architecture, sanitized configs, and working scripts/compose files). Start here, then dive into whichever piece is relevant to you.

---

## 👋 About me

Electronics engineer specialized in communications, 5+ years of experience, currently building and maintaining a self-hosted homelab as a way to keep hands-on with infrastructure, containerization, automation and networking outside of my day job. This repo exists to show *how* I design, document and operate systems — not just that I run them.


---

## 🗺️ Repository map

| Repo | What it covers | Stack |
|---|---|---|
| [`homelab-proxmox-infra`]([../homelab-proxmox-infra](https://github.com/emaro03/homelab-proxmox-infra)) | Hypervisor layer: Proxmox host, LXC container design, storage, backup strategy | Proxmox VE, LXC |
| [`homelab-jellyfin-transcoding`](../homelab-jellyfin-transcoding) | Media server with Intel Quick Sync (QSV) hardware transcoding passthrough into an LXC | Jellyfin, Intel QSV, LXC |
| [`homelab-arr-stack`](../homelab-arr-stack) | Full *arr media-automation stack | Docker Compose, Sonarr/Radarr/Prowlarr/etc. |
| [`homelab-n8n-automation`](../homelab-n8n-automation) | Workflow automation running in the lab | n8n, Docker |
| [`homelab-network-monitoring`](../homelab-network-monitoring) | Uptime monitoring across Cloudflare Tunnels and Tailscale | Uptime Kuma, Cloudflare Tunnel, Tailscale |
| [`homelab-unifi-network`](../homelab-unifi-network) | Network layer: UniFi OS Server, VLANs, topology | UniFi OS Server |
| [`homelab-home-assistant`](../homelab-home-assistant) | Smart home + a Zigbee mesh diagnostic audit (topology, LQI, single points of failure) | Home Assistant, Zigbee (ZHA/Z2M) |

> Each repo can be read on its own — no need to clone all of them to understand one.

---

## 🧠 Design philosophy

A few principles that show up across every repo here:

- **Documented over clever.** Every non-obvious decision has a "why" written down, not just a "what."
- **Reproducible.** Configs are sanitized templates + `.env.example` files, not screenshots — you could stand this up yourself.
- **Iterative.** This lab evolves; each repo's README has a "Status / Known issues" section so you see real engineering trade-offs, not a polished fiction.

---

## 🏗️ High-level architecture

See [`docs/architecture.md`](docs/architecture.md) for the full diagram and write-up of how these pieces fit together (network flow, hypervisor layout, exposure via Cloudflare Tunnel/Tailscale instead of open ports).

---

## 🛠️ Current hardware

| Role | Device | Notes |
|---|---|---|
| Primary host | Dell Optiplex 3040 | i7-6700T (4c), 12GB RAM, 1TB SSD — being evaluated for replacement/upgrade |
| _(add NAS / mini PC here once decided)_ | | Currently evaluating UGREEN DXP2800 GT (NAS consolidation) vs. Lenovo ThinkCentre M920Q (stay on Proxmox) |

---

## 📄 License

Each repo is MIT-licensed unless stated otherwise inside it. Use anything here as a reference for your own lab.
