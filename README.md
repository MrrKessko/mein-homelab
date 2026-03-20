# 🛠 My HomeLab Infrastructure

Übersicht über meine aktuelle Virtualisierungs-Umgebung. Die Infrastruktur ist auf maximale Modularität ausgelegt, wobei **Proxmox VE** als Core-Hypervisor dient.

---

### 🖥 Hypervisor: Proxmox VE
* **Node:** `pve`
* **Status:** Alle kritischen Instanzen laufen stabil.
* **Fokus:** Effizienz und Trennung von Diensten durch LXC und VMs.

### 📦 Virtualisierte Instanzen
| ID | Typ | Name | Zweck |
| :--- | :--- | :--- | :--- |
| **102** | LXC | `docker-host` | Haupt-Host für Docker-Container |
| **104** | LXC | `pihole` | Netzwerkweiter Adblocker (DNS) |
| **106** | LXC | `NextCloud` | Private Cloud Speicher |
| **107** | LXC | `cloudflared` | Sicherer Tunnel-Zugriff |
| **100** | VM | `haos` | Home Assistant (Smart Home) |
| **101** | VM | `AMP` | Game-Server Management |

---

### 📊 Infrastruktur-Diagramm
Hier ist die visuelle Darstellung meines Netzwerks:

```mermaid
graph TD
    classDef hypervisor fill:#212529,stroke:#5c636a,stroke-width:2px,color:white;
    classDef lxc fill:#d4edda,stroke:#155724,color:#155724;
    classDef qemu fill:#cfe2f3,stroke:#105b9b,color:#105b9b;
    
    subgraph PVE["🖥 Proxmox Host"]
        DockerHost(docker-host):::lxc
        Pihole(pihole):::lxc
        Nextcloud(NextCloud):::lxc
        HAOS(Home Assistant):::qemu
    end
    
    DockerHost --> D[15+ Docker Containers]
    Nextcloud --> Storage[(Local Storage)]
