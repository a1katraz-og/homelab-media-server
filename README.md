````markdown
# 🏠 Raspberry Pi 5 Media Server

A production-ready, self-hosted media server stack powered by **Docker Compose** and designed for the **Raspberry Pi 5 (8GB)**.

This repository contains Docker Compose files, application configurations, scripts, and documentation to deploy, maintain, and back up a complete media automation ecosystem.

---

# ✨ Features

- 🎬 Jellyfin Media Server
- 🔍 Jellyseerr Request Management
- 🎞️ Radarr Movie Automation
- 📺 Sonarr TV Show Automation
- 🌐 Prowlarr Indexer Management
- ⬇️ qBittorrent Download Client
- 🔒 Gluetun VPN Gateway (WireGuard/OpenVPN)
- 📝 Bazarr Subtitle Automation *(Optional)*
- 🔄 Watchtower Automatic Updates *(Optional)*
- 🖥️ Portainer Docker Management *(Optional)*
- 📦 Docker Compose Deployment
- 💾 Persistent Volumes
- 🔐 Secure VPN Kill Switch
- 📂 Organized Directory Structure
- 🔄 Easy Backup & Restore
- 🚀 Lightweight and optimized for Raspberry Pi 5

---

# 🏗️ Architecture

```text
                                         Internet
                                             │
                        ┌────────────────────┴────────────────────┐
                        │                                         │
                   Indexers                                 Jellyfin Clients
                        │                                         │
                        ▼                                         ▼
                  +--------------+                      +----------------+
                  |   Prowlarr   |                      |    Jellyfin    |
                  +--------------+                      +----------------+
                          │                                     ▲
             ┌────────────┴────────────┐                        │
             ▼                         ▼                        │
      +--------------+          +--------------+               │
      |    Sonarr    |          |    Radarr    |               │
      +--------------+          +--------------+               │
             │                         │                       │
             └──────────────┬──────────┘                       │
                            │                                  │
                            ▼                                  │
                    +----------------+                         │
                    |  qBittorrent   |                         │
                    +----------------+                         │
                            │                                  │
                network_mode: service:gluetun                  │
                            │                                  │
                            ▼                                  │
                    +----------------+                         │
                    |    Gluetun     |─────────────────────────┘
                    | VPN Gateway    |
                    +----------------+
                            │
                      WireGuard/OpenVPN
                            │
                            ▼
                        VPN Provider
                            │
                            ▼
                         Internet
```

---

# 📁 Repository Structure

```text
media-server/
│
├── README.md
├── LICENSE
├── .env.example
├── docker-compose.yml
│
├── compose/
│   ├── gluetun.yml
│   ├── qbittorrent.yml
│   ├── prowlarr.yml
│   ├── sonarr.yml
│   ├── radarr.yml
│   ├── bazarr.yml
│   ├── jellyfin.yml
│   ├── jellyseerr.yml
│   ├── watchtower.yml
│   └── portainer.yml
│
├── configs/
│   ├── gluetun/
│   ├── qbittorrent/
│   ├── prowlarr/
│   ├── sonarr/
│   ├── radarr/
│   ├── bazarr/
│   ├── jellyfin/
│   ├── jellyseerr/
│   ├── watchtower/
│   └── portainer/
│
├── media/
│   ├── movies/
│   ├── tv/
│   ├── anime/
│   ├── music/
│   └── photos/
│
├── downloads/
│   ├── complete/
│   ├── incomplete/
│   └── torrents/
│
├── backups/
│
├── scripts/
│   ├── backup.sh
│   ├── restore.sh
│   ├── update.sh
│   └── healthcheck.sh
│
└── docs/
    ├── installation.md
    ├── networking.md
    ├── vpn.md
    ├── backups.md
    ├── upgrades.md
    └── troubleshooting.md
```

---

# 🖥️ Hardware

| Component | Recommendation |
|-----------|---------------|
| Raspberry Pi | Pi 5 (8GB) |
| OS | Raspberry Pi OS Lite (64-bit) |
| Storage | 64GB SSD or NVMe |
| Media Storage | External SSD/HDD |
| Cooling | Active Cooler |
| Network | Gigabit Ethernet |
| Power Supply | Official 27W USB-C |

---

# 🐳 Services

| Service | Purpose |
|----------|---------|
| Jellyfin | Media Streaming |
| Jellyseerr | Media Requests |
| Sonarr | TV Automation |
| Radarr | Movie Automation |
| Prowlarr | Indexer Management |
| qBittorrent | Download Client |
| Gluetun | VPN Gateway |
| Bazarr | Subtitle Automation |
| Watchtower | Automatic Updates |
| Portainer | Docker UI |

---

# 🌐 Network Architecture

```text
                    Docker Bridge Network
                            │
     ┌──────────────────────┼──────────────────────────┐
     │                      │                          │
     │                  Jellyfin                  Jellyseerr
     │
     ├── Sonarr
     ├── Radarr
     ├── Prowlarr
     ├── Bazarr
     │
     ├──────────────────────────────────────────────┐
     │                                              │
     ▼                                              ▼
+----------------+                       +-------------------+
|    Gluetun     |<----------------------|   qBittorrent     |
|  VPN Gateway   |   network_mode:service|                   |
+----------------+                       +-------------------+
```

---

# 🔒 VPN Architecture

All torrent traffic passes exclusively through Gluetun.

```text
qBittorrent
      │
      ▼
 Docker Network
      │
      ▼
  Gluetun VPN
      │
Encrypted Tunnel
      │
      ▼
 VPN Provider
      │
      ▼
 Torrent Peers
```

Benefits:

- VPN Kill Switch
- DNS Leak Protection
- IPv6 Leak Protection
- Automatic VPN Reconnect
- Secure Torrent Traffic
- No Direct Internet Access for qBittorrent

---

# 📂 Media Layout

```text
media/

├── movies/
│   ├── Movie Name (2026)
│   │   └── Movie.mkv
│
├── tv/
│   ├── Show Name/
│   │   ├── Season 01/
│   │   └── Season 02/
│
├── anime/
│
├── music/
│
└── photos/
```

---

# 📦 Downloads Layout

```text
downloads/

├── complete/
├── incomplete/
└── torrents/
```

---

# 💾 Volume Mapping

| Host Path | Container Path |
|------------|----------------|
| configs/ | /config |
| downloads/ | /downloads |
| media/movies | /movies |
| media/tv | /tv |
| media/music | /music |

---

# ⚙️ Environment Variables

Create `.env` from `.env.example`

```env
TZ=Asia/Kolkata

PUID=1000
PGID=1000

CONFIG_PATH=/srv/configs
MEDIA_PATH=/srv/media
DOWNLOAD_PATH=/srv/downloads

# Gluetun

VPN_SERVICE_PROVIDER=your-provider
VPN_TYPE=wireguard

WIREGUARD_PRIVATE_KEY=
WIREGUARD_ADDRESSES=

SERVER_COUNTRIES=India
```

---

# 🚀 Installation

## Clone Repository

```bash
git clone https://github.com/<username>/media-server.git

cd media-server
```

---

## Configure Environment

```bash
cp .env.example .env
```

Update the variables according to your environment.

---

## Start the Stack

```bash
docker compose up -d
```

---

## Stop

```bash
docker compose down
```

---

## Restart

```bash
docker compose restart
```

---

## Update Containers

```bash
docker compose pull

docker compose up -d
```

---

# 🔄 Startup Order

1. Gluetun
2. qBittorrent
3. Prowlarr
4. Sonarr
5. Radarr
6. Bazarr
7. Jellyfin
8. Jellyseerr
9. Watchtower
10. Portainer

---

# 🔗 Service Integration

```text
Indexers
    │
    ▼
Prowlarr
    │
    ├──────────────┐
    ▼              ▼
Sonarr         Radarr
    │              │
    └──────┬───────┘
           ▼
    qBittorrent
           │
     (via Gluetun)
           │
           ▼
      Downloads
           │
           ▼
     Media Library
           │
           ▼
      Jellyfin
           ▲
           │
      Jellyseerr
```

---

# 🛠️ Useful Docker Commands

Running Containers

```bash
docker ps
```

Container Logs

```bash
docker logs jellyfin
```

```bash
docker logs qbittorrent
```

```bash
docker logs gluetun
```

Restart One Service

```bash
docker compose restart jellyfin
```

Pull Latest Images

```bash
docker compose pull
```

Recreate Containers

```bash
docker compose up -d
```

Docker Disk Usage

```bash
docker system df
```

Clean Unused Images

```bash
docker image prune
```

Disk Usage

```bash
df -h
```

---

# 💾 Backup Strategy

Backup regularly:

- configs/
- docker-compose.yml
- .env
- scripts/
- databases
- media (optional depending on storage)

Recommended schedule:

- Daily configuration backups
- Weekly full backups
- Monthly offsite backup

---

# 🔐 Security Recommendations

- Never expose qBittorrent directly to the internet.
- Route all torrent traffic through Gluetun.
- Use WireGuard whenever supported.
- Keep all Docker images updated.
- Use strong passwords.
- Enable HTTPS through a reverse proxy.
- Restrict external access.
- Store secrets in `.env`.
- Schedule regular backups.
- Keep Raspberry Pi OS updated.

---

# 🚀 Future Improvements

- [ ] Traefik Reverse Proxy
- [ ] Nginx Proxy Manager
- [ ] Homepage Dashboard
- [ ] Tailscale Remote Access
- [ ] Cloudflare Tunnel
- [ ] Grafana
- [ ] Prometheus
- [ ] Loki
- [ ] Uptime Kuma
- [ ] Immich
- [ ] Nextcloud
- [ ] Automatic Backups
- [ ] SSD Health Monitoring
- [ ] UPS Monitoring
- [ ] Discord Notifications
- [ ] Telegram Notifications

---

# ❗ Troubleshooting

## Container won't start

```bash
docker logs <container-name>
```

## Permission Issues

```bash
sudo chown -R 1000:1000 configs/
```

Verify:

- PUID
- PGID
- Folder Ownership

---

## Jellyfin Cannot See Media

Verify:

- Volume mappings
- Folder permissions
- Library configuration

---

## Sonarr/Radarr Import Issues

Check:

- Download paths
- Remote Path Mapping
- Completed Download Handling

---

## VPN Not Connected

```bash
docker logs gluetun
```

Verify:

- WireGuard credentials
- VPN Provider
- Firewall rules
- Internet connectivity

---

# 📈 Resource Usage (Typical)

| Service | RAM | CPU |
|----------|----:|----:|
| Jellyfin | 300–500 MB | Low |
| Jellyseerr | 150–250 MB | Very Low |
| Sonarr | 250–400 MB | Low |
| Radarr | 250–400 MB | Low |
| Prowlarr | 150–250 MB | Very Low |
| qBittorrent | 150–300 MB | Low |
| Gluetun | 50–100 MB | Very Low |
| Docker Overhead | ~200 MB | Minimal |

**Typical idle RAM usage:** **2–3 GB**

---

# 📄 License

MIT License

---

# ❤️ Acknowledgements

Special thanks to the communities behind:

- Jellyfin
- Jellyseerr
- Sonarr
- Radarr
- Prowlarr
- qBittorrent
- Gluetun
- Bazarr
- LinuxServer.io
- Docker
- Raspberry Pi Foundation
````
