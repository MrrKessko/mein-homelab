[mein-homelab_updated.md](https://github.com/user-attachments/files/28360532/mein-homelab_updated.md)
# 🔧 mein-homelab

> Persönliche Homelab-Infrastruktur auf Basis von **Proxmox VE 9.2.2** — modular, containerisiert und selbst gehostet.

![Proxmox](https://img.shields.io/badge/Proxmox-VE%209.2.2-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-29.4.3-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-AMD%20Ryzen%205%203550H-ED1C24?style=for-the-badge&logo=amd&logoColor=white)
![Power](https://img.shields.io/badge/Strom-~12W-2EA043?style=for-the-badge&logo=power&logoColor=white)

---

## 📋 Inhalt

- [Hardware](#-hardware)
- [Proxmox — Übersicht](#-proxmox--übersicht)
- [LXC Container](#-lxc-container)
- [Virtuelle Maschinen](#-virtuelle-maschinen)
- [Docker Services (LXC 102)](#-docker-services-lxc-102)
- [Docker Services (VM 103)](#-docker-services-vm-103)
- [Netzwerk & Zugang](#-netzwerk--zugang)
- [Stromverbrauch](#-stromverbrauch)
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
| **OS** | Proxmox VE 9.2.2 (Bare-Metal) |
| **Strom** | ~12W Gesamtaufnahme gemessen (CPU allein ~6,2W) |

---

## 🏗️ Proxmox — Übersicht

```
192.168.178.94 ── Proxmox Host (BOSGAME E4)
       │
       ├── LXC 102  docker-host    (192.168.178.50)  ← Haupt-Docker
       ├── LXC 104  pihole         (192.168.178.104) ← DNS/Adblock
       ├── LXC 106  nextcloud      (192.168.178.106) ← Cloud
       ├── LXC 107  cloudflared    (192.168.178.111) ← Tunnel
       ├── VM 100   Home Assistant ← Smart Home
       ├── VM 101   AMP-GameServer ← Games (gestoppt)
       └── VM 103   docker-srv     (192.168.178.139) ← Hermes + Tools
```

---

## 📦 LXC Container

| ID | Name | IP | Typ | Beschreibung |
|---|---|---|---|---|
| 102 | docker-host | 192.168.178.50 | LXC | Haupt-Docker-Host — alle containerisierten Dienste |
| 104 | pihole | 192.168.178.104 | LXC | Netzwerkweiter Adblocker via DNS |
| 106 | nextcloud | 192.168.178.106 | LXC | Self-hosted Cloud-Speicher & Datei-Sync |
| 107 | cloudflared | 192.168.178.111 | LXC | Cloudflare Tunnel für sicheren externen Zugriff |

---

## 🖱️ Virtuelle Maschinen

| ID | Name | IP | RAM | Disk | Typ | Beschreibung |
|---|---|---|---|---|---|---|
| 100 | haos-16.3 | — | 4 GB | 32 GB | QEMU | Home Assistant OS — Smart Home Automation |
| 101 | AMP-GameServer | — | 8 GB | 140 GB | QEMU | Game-Server-Verwaltung (derzeit gestoppt) |
| 103 | docker-srv | 192.168.178.139 | 6 GB | 64 GB | QEMU | Hermes Agent + IQ-System + Tools |

---

## 🐳 Docker Services (LXC 102)

Alle primären Dienste laufen auf **LXC 102 (docker-host, 192.168.178.50)**, verwaltet über [Dockhand](https://github.com/fmys/dockhand).

Gesamt: **23 aktive Container** auf dem Host.

### 🔒 Security & Passwörter

| Container | Image | Beschreibung |
|---|---|---|
| vaultwarden | vaultwarden/server:latest | Self-hosted Bitwarden-kompatibler Passwortmanager |

### 📊 Monitoring & Management

| Container | Image | Beschreibung |
|---|---|---|
| dockhand | fnsys/dockhand:latest | Leichtgewichtige Docker-Management-UI |
| dozzle | amir20/dozzle:latest | Echtzeit Docker Log Viewer |
| netalertx | ghcr.io/jokob-sk/netalertx:latest | Netzwerk-Geräte & Eindringlingsalarme |

### 🛠️ Tools & Utilities

| Container | Image | Beschreibung |
|---|---|---|
| cyberchef | mpepping/cyberchef:latest | Web-basiertes Daten-Analyse & Encoding-Tool |
| it-tools | corentinth/it-tools:latest | Sammlung nützlicher IT-Utilities |
| librespeed | lscr.io/linuxserver/librespeed:latest | Self-hosted Speedtest |
| changedetection | ghcr.io/dgtlmoon/changedetection.io:latest | Website-Änderungsüberwachung |

### 📡 Benachrichtigungen & Downloads

| Container | Image | Beschreibung |
|---|---|---|
| ntfy | binwiederhier/ntfy:latest | Self-hosted Push-Notification-Service |
| download-notifier | alpine:latest | Download-Abschluss-Benachrichtiger |
| server-1 | ghcr.io/alexta69/metube:latest | MeTube Download-Server Instanz 1 |
| server-2 | ghcr.io/alexta69/metube:latest | MeTube Download-Server Instanz 2 |
| server-3 | ghcr.io/alexta69/metube:latest | MeTube Download-Server Instanz 3 |
| server-4 | ghcr.io/alexta69/metube:latest | MeTube Download-Server Instanz 4 |
| server-auswahl | nginx:alpine | Nginx Reverse Proxy / Server-Selektor |

### 🧩 New Additions (seit letztem Update)

| Container | Image | Beschreibung |
|---|---|---|
| sillytavern | ghcr.io/sillytavern/sillytavern:latest | KI-Charakter-Interface |
| paperless-ngx | ghcr.io/paperless-ngx/paperless-ngx:latest | Dokumenten-Management-System |
| paperless-db | postgres:16 | PostgreSQL für Paperless-ngx |
| paperless-redis | redis:7 | Redis-Cache für Paperless-ngx |

### ❌ Entfernt (seit letztem Update)

- ~~uptime-kuma~~ — durch andere Lösungen abgelöst
- ~~portainer~~ — Verwaltung läuft über Dockhand
- ~~homepage~~ — nicht mehr aktiv genutzt

---

## 🐳 Docker Services (VM 103)

Auf **VM 103 (docker-srv, 192.168.178.139)** laufen ergänzende Dienste:

| Container | Image | Beschreibung |
|---|---|---|
| iq-web-01 | nginx:alpine | IQ-System Web-Frontend |
| iq-db-01 | mariadb:10 | IQ-System Datenbank |
| iq-ssh | linuxserver/openssh-server | SSH-Gateway |
| clawvin-tools | corentinth/it-tools:latest | IT-Utilities für den Hermes-Agenten |

Auf dieser VM läuft außerdem der **Hermes AI Agent** (DeepSeek Flash / Claude), der als persönlicher Assistent, Systemadministrator und Entwickler fungiert.

---

## 🌐 Netzwerk & Zugang

| Funktion | Lösung | Details |
|---|---|---|
| DNS & Adblocking | Pi-hole (LXC 104) | 192.168.178.104 |
| Externer Zugriff | Cloudflare Tunnel | LXC 107 — keine offenen Inbound-Ports |
| Netzwerk-Überwachung | NetAlertX | Docker-Container auf LXC 102 |
| Monitoring | Hermes Cron + NTFY | Tägliche Reports + Push-Benachrichtigungen |
| Remote-Zugriff | iq-ssh (VM 103) | OpenSSH-Server |

---

## ⚡ Stromverbrauch

Gemessen am 28. Mai 2026 via `powerstat -R` auf dem Proxmox-Host:

| Messung | Wert |
|---|---|
| **CPU (RAPL)** | ~6,2 W Durchschnitt bei 96,7 % Idle |
| **Gesamtsystem (geschätzt)** | ~11–14 W |
| **Jahresverbrauch** | ~100–120 kWh |
| **Kosten (bei ~30 ct/kWh)** | ~30–40 € pro Jahr |

Der BOSGAME E4 ist mit seinem 35W-Netzteil für den 24/7-Dauerbetrieb ausgelegt.

---

## 💻 Daily Driver

| Komponente | Details |
|---|---|
| **OS** | Pop!_OS 24.04 (COSMIC + GNOME X11) |
| **CPU** | Intel Core i7-8700 |
| **Mainboard** | ASUS ROG Strix Z390-F |
| **RAM** | 32 GB Corsair Vengeance LPX (4× 8 GB) |
| **GPU** | NVIDIA GeForce RTX 2070 Super (8 GB) |

---

*Stand: Mai 2026 — frisch gemessen und aktualisiert.*
