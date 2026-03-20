# 🔧 mein-homelab

> Persönliche Homelab-Infrastruktur auf Basis von **Proxmox VE 9.1.4** — modular, containerisiert und selbst gehostet.

![Proxmox](https://img.shields.io/badge/Proxmox-VE%209.1.4-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-29.2.1-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Uptime](https://img.shields.io/badge/Uptime-Kuma%20monitored-brightgreen?style=for-the-badge&logo=statuspage&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-AMD%20Ryzen%205%203550H-ED1C24?style=for-the-badge&logo=amd&logoColor=white)

---

## 📋 Inhalt

- [Hardware](#-hardware)
- [Proxmox — Übersicht](#-proxmox--übersicht)
- [LXC Container](#-lxc-container)
- [Virtuelle Maschinen](#-virtuelle-maschinen)
- [Docker Services](#-docker-services)
- [Netzwerk & Zugang](#-netzwerk--zugang)
- [Daily Driver](#-daily-driver)

---

## 🖥️ Hardware

### Proxmox-Server — BOSGAME E4

| Komponente | Details |
|---|---|
| **Modell** | BOSGAME E4 Mini-PC |
| **CPU** | AMD Ryzen 5 3550H · 4 Kerne · 3,7 GHz |
| **RAM** | 16 GB DDR4 @ 2400 MHz |
| **Storage** | 512 GB PCIe NVMe SSD |
| **GPU** | AMD Radeon Vega 8 (integriert) |
| **OS** | Proxmox VE 9.1.4 (Bare-Metal) |

---

## 🏗️ Proxmox — Übersicht


---

## 📦 LXC Container

| ID | Name | Typ | Beschreibung |
|---|---|---|---|
| 102 | `docker-host` | LXC | Haupt-Docker-Host — läuft alle containerisierten Dienste |
| 104 | `pihole` | LXC | Netzwerkweiter Adblocker via DNS |
| 106 | `nextcloud` | LXC | Self-hosted Cloud-Speicher & Datei-Sync |
| 107 | `cloudflared` | LXC | Cloudflare Tunnel für sicheren externen Zugriff |

---

## 🖱️ Virtuelle Maschinen

| ID | Name | Typ | Beschreibung |
|---|---|---|---|
| 100 | `haos-16.3` | QEMU | Home Assistant OS — Smart Home Automation |
| 101 | `AMP-GameServer` | QEMU | Game-Server-Verwaltung via AMP |
| 103 | `docker-srv` | QEMU | Sekundärer Docker-Host |

---

## 🐳 Docker Services

Alle primären Dienste laufen in **LXC 102 (docker-host)**, verwaltet über [Dockhand](https://github.com/fmys/dockhand).

### 🔒 Security & Passwörter

| Container | Image | Port | Beschreibung |
|---|---|---|---|
| `vaultwarden` | `vaultwarden/server:latest` | `8088→80` | Self-hosted Bitwarden-kompatibler Passwortmanager |

### 📊 Monitoring & Management

| Container | Image | Port | Beschreibung |
|---|---|---|---|
| `uptime-kuma` | `louislam/uptime-kuma` | `3001→3001` | Service-Uptime-Monitoring |
| `dozzle` | `amir20/dozzle:latest` | `3018→8080` | Echtzeit Docker Log Viewer |
| `dockhand` | `fmys/dockhand:latest` | `2999→3000` | Leichtgewichtige Docker-Management-UI |
| `portainer` | `portainer/portainer-ce:latest` | `9443→9443` | Vollständige Docker-Management-UI |
| `netalertx` | `ghcr.io/jokob-sk/netalertx:latest` | — | Netzwerk-Geräte & Eindringlingsalarme |
| `homepage` | `ghcr.io/gethomepage/homepage:latest` | `3000→3000` | Anpassbares Homelab-Dashboard |

### 🛠️ Tools & Utilities

| Container | Image | Port | Beschreibung |
|---|---|---|---|
| `cyberchef` | `mpepping/cyberchef:latest` | `3014→8000` | Web-basiertes Daten-Analyse & Encoding-Tool |
| `it-tools` | `corentinth/it-tools:latest` | `8080→80` | Sammlung nützlicher IT-Utilities |
| `librespeed` | `lscr.io/linuxserver/librespeed:latest` | `3015→80` | Self-hosted Speedtest |
| `n8n` | `n8nio/n8n` | `5678→5678` | Workflow-Automatisierungsplattform |

### 📡 Benachrichtigungen & Downloads

| Container | Image | Port | Beschreibung |
|---|---|---|---|
| `ntfy` | `binwiederhier/ntfy:latest` | `3017→80` | Self-hosted Push-Notification-Service |
| `download-notifier` | `alpine:latest` | — | Download-Abschluss-Benachrichtiger |
| `server-1` | `ghcr.io/alexa69/metube:latest` | `3035→8081` | MeTube Download-Server Instanz 1 |
| `server-2` | `ghcr.io/alexa69/metube:latest` | `3044→8081` | MeTube Download-Server Instanz 2 |
| `server-3` | `ghcr.io/alexa69/metube:latest` | `3045→8081` | MeTube Download-Server Instanz 3 |
| `server-4` | `ghcr.io/alexa69/metube:latest` | `3047→8081` | MeTube Download-Server Instanz 4 |
| `server-auswahl` | `nginx:alpine` | `3046→80` | Nginx Reverse Proxy / Server-Selektor |

---

## 🌐 Netzwerk & Zugang


| Funktion | Lösung |
|---|---|
| DNS & Adblocking | Pi-hole (LXC 104) |
| Externer Zugriff | Cloudflare Tunnel — keine offenen Inbound-Ports |
| Uptime-Monitoring | Uptime Kuma |
| Netzwerk-Überwachung | NetAlertX |
| Docker-Subnetz | `172.x.x.x` (Bridge Mode) |

---

## 💻 Daily Driver

| Komponente | Details |
|---|---|
| **OS** | Pop!_OS mit COSMIC Desktop |
| **CPU** | Intel Core i7-8700 |
| **Mainboard** | ASUS ROG Strix Z390-F |
| **RAM** | 32 GB Corsair Vengeance LPX (4× 8 GB) |
| **GPU** | NVIDIA GeForce GTX 1060 |

---

## 📁 Repo-Struktur


---

<div align="center">
  <sub>🏠 Hosted in Jülich, Germany · Zugang ausschließlich via Cloudflare Tunnel</sub>
</div>
