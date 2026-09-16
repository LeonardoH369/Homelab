# Homelab — Multi-Layer Infrastructure with Docker, pfSense, VLANs, and Monitoring

[![Status](https://img.shields.io/badge/status-en%20producci%C3%B3n-brightgreen)](#) [![Docker](https://img.shields.io/badge/Docker-Portainer-2496ED?logo=docker&logoColor=white)](#) [![pfSense](https://img.shields.io/badge/Firewall-pfSense-212121?logo=pfsense&logoColor=white)](#) [![Network](https://img.shields.io/badge/Switch-Cisco%203750X-1BA0D7?logo=cisco&logoColor=white)](#)

A personal homelab with a multi-layer architecture (dedicated firewall, managed switch, container host, and virtualization server), designed to replicate real enterprise/cloud operation patterns: network segmentation, secure remote access, real-time monitoring, and centralized service management.

This project grew out of the need for a personal environment to practice Linux/Windows Server administration, networking (including real Cisco hardware), and observability — skills directly applicable to Networking, Cloud, and Cybersecurity roles.

## Table of Contents

- [Architecture](#architecture)
- [Infrastructure Components](#infrastructure-components)
- [Deployed Services](#deployed-services-raspberry-pi-5--docker)
- [Virtualization](#virtualization-main-pc)
- [Networking & Segmentation](#networking--segmentation)
- [Technical Challenges](#technical-challenges-and-how-i-solved-them)
- [Future Improvements](#future-improvements)
- [Technologies](#technologies)
- [Author](#author)

---

## Architecture

```
Internet
   │
ISP Router
   │
pfSense (Firewall) ── Dedicated PC with dual-port NIC
   │
Cisco Catalyst 3750X Switch (24 Gigabit ports) ── network management / VLANs
   │
   ├── Raspberry Pi 5 (8GB, static IP) ── Docker container host
   │       └── Portainer, Pi-hole, Tailscale, Heimdall, Jellyfin, Navidrome, Grafana
   │
   └── Main PC ── virtualization host
           ├── Windows Server VM (Active Directory / GPOs)
           └── Ubuntu Server VM
```

The homelab is segmented from the regular home network, with pfSense acting as the control point between the two and the managed 3750X switch handling internal traffic.

---

## Infrastructure Components

| Layer                    | Component                                  | Function                                                          |
| ------------------------- | ------------------------------------------ | ------------------------------------------------------------------ |
| Firewall                  | pfSense (PC with dual-port NIC)            | Traffic filtering, segmentation between homelab and home network  |
| Switching                 | Cisco Catalyst 3750X — 24 Gigabit ports    | Internal network management, console/SSH administration           |
| Compute (containers)      | Raspberry Pi 5 (8GB RAM, static IP)        | Main host for all Dockerized services                             |
| Compute (virtualization)  | Main PC                                    | Runs Windows Server and Ubuntu Server VMs                         |
| Remote connectivity       | ISP Router                                 | Internet uplink                                                   |

---

## Deployed Services (Raspberry Pi 5 + Docker)

| Service    | Function                                                                  |
| ---------- | -------------------------------------------------------------------------- |
| Portainer  | Visual management and administration of Docker containers/stacks         |
| Docker Hub | Image registry used for the homelab's containers                        |
| Pi-hole    | Local DNS + network-wide ad/tracker blocking                             |
| Tailscale  | Mesh VPN (WireGuard) for secure remote access without exposing public ports |
| Heimdall   | Centralized dashboard for accessing all homelab services                 |
| Jellyfin   | Media streaming server                                                    |
| Navidrome  | Self-hosted music server                                                  |
| Grafana    | Metrics visualization and monitoring dashboards                          |

---

## Virtualization (Main PC)

- Windows Server VM — Active Directory Domain Services (AD DS): users, groups, OUs, and GPOs
- Ubuntu Server VM — additional testing/development environment

---

## Networking & Segmentation

- Homelab segmented from the main home network via pfSense
- Cisco Catalyst 3750X switch managing internal homelab traffic
- Raspberry Pi 5 with a static IP to ensure consistent service availability

---

## Technical Challenges (and How I Solved Them)

**Port conflict between containers** — Pi-hole and another service were both trying to use the same port (8080), causing one of them to fail to start correctly. I solved this by remapping the exposed port of one container in `docker-compose.yml`, which required understanding the difference between a container's internal port and the port published to the host.

**Initial access to the server (Raspberry Pi 5)** — Setting up remote SSH access to the RPi5 was a challenge at first, from the initial connection setup to making it stable enough for remote administration. Once solved, this became the foundation for managing the entire homelab without a monitor/keyboard directly connected to the Pi.

---

## Future Improvements

- [ ] Add Prometheus + Node Exporter as a metrics source for Grafana
- [ ] Automate configuration backups with a script
- [ ] Document pfSense firewall rules
- [ ] Configure additional VLANs on the 3750X switch to further isolate services
- [ ] Add a reverse proxy (Nginx Proxy Manager / Traefik) with internal TLS certificates

---

## Technologies

`Docker` `Portainer` `Raspberry Pi 5` `pfSense` `Cisco Catalyst 3750X` `Grafana` `Pi-hole` `Tailscale` `Windows Server` `Active Directory` `Ubuntu Server` `SSH`

---

## Author

**Leonardo Hinojosa Castro**
Software Development Engineering Student — Tecmilenio
[LinkedIn](https://www.linkedin.com/in/leonardo-hinojosa-castro-323548224/)
