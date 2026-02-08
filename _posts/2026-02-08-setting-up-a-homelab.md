---
layout: post
title: "Setting Up a Homelab: A Practical Guide"
date: 2026-02-08
categories: homelab
---

If you've ever wanted full control over your own infrastructure, a homelab is the way to go. It's a space to learn, experiment, and run services that you actually depend on day to day. Here's how I think about setting one up.

## Why Bother?

A homelab gives you hands-on experience with technologies that are hard to learn from documentation alone. Networking, storage, virtualization, containers, DNS, certificates — you'll touch all of it. It's also a great way to self-host services and stop relying on third-party platforms for everything.

## Picking Hardware

You don't need enterprise gear to start. A single machine with decent RAM goes a long way.

**Good starting points:**
- An old desktop or laptop with 16+ GB of RAM
- A mini PC (Intel NUC, Lenovo ThinkCentre Tiny, Dell OptiPlex Micro)
- Used enterprise servers if you don't mind the noise and power draw (Dell PowerEdge, HP ProLiant)

**What matters most:**
- **RAM** — virtualization and containers eat memory fast
- **Storage** — SSDs for OS and workloads, HDDs for bulk storage
- **CPU** — most modern CPUs are fine; cores matter more than clock speed
- **Network** — gigabit minimum, 2.5G if your switch supports it

## Choosing a Hypervisor

You need a way to run multiple workloads on one box. The main options:

| Option | Notes |
|--------|-------|
| **Proxmox VE** | Free, Debian-based, good web UI, handles VMs and containers |
| **KVM/libvirt** | Bare-metal Linux with QEMU/KVM, maximum flexibility |
| **ESXi** | VMware's hypervisor, solid but licensing has changed |
| **XCP-ng** | Open-source Xen, good if you want Xen Orchestra for management |

I'd recommend **Proxmox** for most people starting out. It has a low barrier to entry and a large community.

## Networking Basics

At minimum, you want:

- A managed switch so you can set up VLANs
- A dedicated subnet for lab traffic
- A local DNS server (Pi-hole, AdGuard Home, or a simple BIND/dnsmasq setup)
- A reverse proxy (Nginx Proxy Manager, Traefik, or Caddy) for routing traffic to services

Separate your lab network from your home network early. It saves headaches later.

## Essential Services to Run First

Once you have a hypervisor running, start with these:

1. **DNS** — Local DNS resolution makes everything else easier
2. **Reverse proxy** — Access services by name instead of IP:port
3. **Docker host** — A VM or LXC container running Docker for lightweight services
4. **Monitoring** — Grafana + Prometheus or a simple Uptime Kuma instance
5. **Backups** — Proxmox Backup Server, Restic, or Borg

## Container Orchestration

Once you outgrow single-node Docker, consider:

- **Docker Compose** — Good enough for a single host with many services
- **K3s** — Lightweight Kubernetes, great for homelabs
- **RKE2** — More enterprise-focused Kubernetes if you want closer-to-production experience

Kubernetes is overkill for most homelab setups, but if learning it is the goal, K3s on a few VMs is the sweet spot.

## Storage

For anything beyond a single disk:

- **ZFS** — Built-in to Proxmox, great data integrity, snapshots, and compression
- **LVM** — Simpler, works well for basic VM storage
- **NFS/SMB shares** — For shared storage across VMs and containers
- **Longhorn or Rook-Ceph** — If you're running Kubernetes and need persistent volumes

Start with ZFS mirrors. They're simple, fast, and protect against single-disk failures.

## Tips I Wish I Knew Earlier

- **Document everything.** You will forget why you configured something six months from now.
- **Use infrastructure as code.** Ansible, Terraform, or even just shell scripts. Clickops doesn't scale.
- **Set up backups before you need them.** Test restores too.
- **Don't over-buy hardware.** Start small and expand when you hit real limits.
- **Power consumption adds up.** A server running 24/7 at 200W costs real money over a year.
- **UPS is not optional.** A cheap UPS protects your gear and gives you clean shutdowns.

## What's Next

In future posts, I'll dig into specific parts of my setup in more detail — networking, Kubernetes clusters, and self-hosted services. The homelab is never really "done," and that's part of the fun.
