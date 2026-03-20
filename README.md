🛠 My HomeLab InfrastructureÜbersicht über meine aktuelle Virtualisierungs-Umgebung und die darauf laufenden Dienste. Die Infrastruktur ist auf maximale Modularität ausgelegt, wobei Proxmox VE als Core-Hypervisor dient.🖥 Hypervisor: Proxmox VENode: pveStatus: Alle kritischen Instanzen laufen stabil (Uptime > 2 Tage).Ressourcen-Auslastung: Moderate CPU-Last, Fokus liegt auf RAM-Effizienz für die Virtualisierung.📦 Virtualisierte Instanzen (VMs & LXC)IDTypNameZweck102LXCdocker-hostHaupt-Maschinenraum für Container-Workloads104LXCpiholeNetzwerkweiter Adblocker & DNS (Tag: adblock)106LXCNextCloudPrivate Cloud-Speicherlösung107LXCcloudflaredSicherer Tunnel für Remote-Zugriff100VMhaos-16.3Home Assistant OS (Smart Home Zentrale)101VMAMP-GameServerGame-Server Management (CubeCoders AMP)103VMdocker-srvSekundärer Docker-Service Host🐳 Docker Stack (Running on docker-host)Die Container-Infrastruktur wird via Dockhand (und Portainer) verwaltet. Aktuell sind 15 Container aktiv:Core Services & ToolsSecurity: vaultwarden (Passwort-Management)Networking: netalertx (Intrusion Detection), librespeed (Speedtest)Monitoring: dozzle (Log-Viewer), dockhand (Management GUI)Utility: cyberchef (The Cyber Swiss Army Knife), it-tools (Dev-Tool Sammlung)Media & AutomationDownload Stack: Mehrere metube Instanzen (server-1 bis server-4) für Video-Downloads, gesteuert über eine server-auswahl.Notifications: ntfy für Echtzeit-Benachrichtigungen und download-notifier.🔧 Tech StackVirtualisierung: Proxmox VEOS: Debian/Ubuntu (LXC), Home Assistant OSContainer: Docker & Docker ComposeManagement: Dockhand, Portainer, Cloudflare Tunnels




graph TD
    %% Define Styles
    classDef hypervisor fill:#212529,stroke:#5c636a,stroke-width:2px,color:white;
    classDef lxc fill:#d4edda,stroke:#155724,color:#155724;
    classDef qemu fill:#cfe2f3,stroke:#105b9b,color:#105b9b;
    classDef dockerfill fill:#17223b,stroke:#005cbf,stroke-width:2px,color:#cfe2f3;
    classDef dockercore fill:#444,stroke:#777,stroke-dasharray: 5 5,color:white;
    classDef dockermedia fill:#333,stroke:#666,stroke-dasharray: 5 5,color:white;

    subgraph PROXMOX_SERVER["🖥 Proxmox VE 8.x Host ('pve')"]
        direction TB

        %% LXC Containers
        subgraph LXC_SUBSYSTEM["📦 LXC Containers"]
            DockerHost_LXC(ID: 102 - <b>docker-host</b><br/>Major Workload):::lxc
            Pihole(ID: 104 - <b>pihole</b><br/>DNS/Adblock):::lxc
            Nextcloud(ID: 106 - <b>NextCloud</b><br/>Storage):::lxc
            Cloudflared(ID: 107 - <b>cloudflared</b><br/>Tunnels):::lxc
        end

        %% QEMU VMs
        subgraph QEMU_SUBSYSTEM["🖥 VMs"]
            HAOS(ID: 100 - <b>haos</b><br/>Smart Home OS):::qemu
            AMP_Game(ID: 101 - <b>AMP-GameServer</b>):::qemu
            DockerSrv_VM(ID: 103 - <b>docker-srv</b>):::qemu
        end
    end

    %% Internal Networking Structure
    subgraph DOCKER_INFRA["🐳 Docker Stack on 'docker-host'"]
        direction TB
        Dockhand_UI[<b>Dockhand</b> (Mgmt)]:::dockercore
        NetAlertx[<b>netalertx</b> (IDS)]:::dockercore
        Vaultwarden[<b>vaultwarden</b> (Passwords)]:::dockercore
        Metube_Cluster[<b>metube</b><br/>x4 Nodes]:::dockermedia
        Ntfy[<b>ntfy</b> (Alarms)]:::dockercore
    end

    %% Connections
    PROXMOX_SERVER ===> DockerHost_LXC
    PROXMOX_SERVER ===> Pihole
    PROXMOX_SERVER ===> Nextcloud
    PROXMOX_SERVER ===> Cloudflared
    PROXMOX_SERVER ===> HAOS
    PROXMOX_SERVER ===> AMP_Game
    PROXMOX_SERVER ===> DockerSrv_VM

    %% Define that DockerHost hosts the Docker services
    DockerHost_LXC -. Hosts .-> DOCKER_INFRA

    %% Connectivity
    Cloudflared -.. External_Access(Remote Access Tunnels):::qemu
