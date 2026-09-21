# Architecture Overview

## Goals

- Keep services self-hosted and under my own control.
- Avoid exposing ports directly to the internet — everything external goes through Cloudflare Tunnel or Tailscale.
- Keep the hypervisor layer (Proxmox) separate and recoverable from any single service failing.
- Monitor availability of every externally-reachable service from outside the LAN.

## Diagram

```
                          ┌─────────────────────────┐
                          │        Internet         │
                          └─────────────┬───────────┘
                                        │
                     ┌──────────────────┴──────────────────┐
                     │                                     │
             ┌───────▼────────┐                  ┌─────────▼─────────┐
             │ Cloudflare     │                  │ Tailscale         │
             │ Tunnel         │                  │ (private mesh VPN)│
             └───────┬────────┘                  └─────────┬─────────┘
                     │                                     │
                     └──────────────────┬──────────────────┘
                                        │
                          ┌─────────────▼─────────────┐
                          │   UniFi OS Server / VLANs │
                          └─────────────┬─────────────┘
                                        │
                          ┌─────────────▼──────────────┐
                          │      Proxmox VE Host       │
                          │ (Optiplex 3040 — i7-6700T) │
                          └──────────────┬─────────────┘
                                         │
        ┌─────────────────┬──────────────┼───────────────┬────────────────┐
        │                 │              │               │                │
 ┌──────▼───────┐  ┌──────▼──────┐ ┌─────▼──────┐ ┌───────▼──────┐ ┌───────▼──────┐
 │ Jellyfin LXC │  │  *arr stack │ │    n8n     │ │ Uptime Kuma  │ │Home Assistant│
 │ + QSV passthr│  │  (Docker)   │ │  (Docker)  │ │ (Docker)     │ │+ Zigbee mesh │
 └──────────────┘  └─────────────┘ └────────────┘ └──────────────┘ └──────────────┘
```

## Layers

1. **Access layer** — Cloudflare Tunnel (public-facing services) and Tailscale (private/admin access). No ports forwarded on the router.
2. **Network layer** — UniFi OS Server manages VLANs, segmenting IoT/lab/admin traffic.
3. **Hypervisor layer** — Proxmox VE on the primary host, mixing LXC containers (lightweight, for services that benefit from direct hardware access like Jellyfin+QSV) and Docker Compose stacks (for everything else).
4. **Service layer** — Jellyfin (media), *arr stack (media automation), n8n (workflow automation), Uptime Kuma (monitoring).

## Key trade-offs (and why)

- **LXC vs. Docker VM:** Jellyfin runs in an LXC with QSV passthrough instead of a Docker container inside a VM, because passing an iGPU through to an LXC is lower-overhead than through a full VM — worth the extra setup complexity for transcoding performance.
- **Tunnel/mesh VPN over port forwarding:** trades a small amount of latency/complexity for a meaningfully smaller attack surface — no inbound ports open on the WAN side at all.
- **Currently under evaluation:** whether to consolidate everything onto a single NAS (UGREEN DXP2800 GT) vs. staying on Proxmox with a more capable mini PC (Lenovo ThinkCentre M920Q). Trade-off is operational simplicity vs. flexibility — see [`homelab-proxmox-infra`](https://github.com/emaro03/homelab-proxmox-infra) for the live decision log.

## Status

This is a living lab, not a finished product. Known rough edges are tracked in each repo's own README under "Status / Known issues" — e.g. slow boot/startup times after restarts, currently being investigated.
