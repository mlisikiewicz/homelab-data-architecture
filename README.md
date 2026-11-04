# Personal Enterprise Data & Infrastructure Lab

## 🌐 Overview
A self-hosted, highly available homelab environment designed to simulate enterprise infrastructure, automated data pipelines, and segregated network security. This project serves as a testing ground for container orchestration, database integrity, and efficient storage lifecycle management.

---

## 🏗️ System Architecture & Hardware
To ensure high performance and protect system drives from excessive write wear, the infrastructure separates core hypervisors from bulk data storage:
* **Hypervisor:** Proxmox VE running on a dedicated Dell Optiplex node.
* **Storage Allocation:** System boot volumes are isolated from heavy input/output tasks, routing all container appdata, logs, and media databases directly to a high-speed **2TB secondary internal SSD**.
* **Containerization:** Lightweight Linux Containers (LXCs) host isolated services to maintain clean boundaries between media streaming, network automation, and data indexing.

---

## 🔒 Network Topology & Security (UniFi Stack)
Network segmentation is critical for isolating IoT devices from core data traffic. The infrastructure is managed via a UniFi gateway and switch ecosystem:
* **Primary VLAN ("Free Wi-Fi"):** Dedicated trusted network for primary user devices and workstations.
* **Isolated IoT VLAN:** Restricts smart home hardware, printers, and background appliances from accessing internal server shares.
* **Local DNS & Ad-Blocking:** Implements network-wide DNS filtering at the gateway level to optimize bandwidth and secure local endpoints.

---

## 📊 Data Pipelines & Information Management
Just like a supply chain inventory system, a robust homelab requires strict data governance, automated tracking, and clean metadata synchronization:
* **Automated Indexing & Metadata Enrichment:** Manages structured relational metadata for thousands of media and document assets, resolving automated ID mismatches and enforcing strict folder hierarchies.
* **Backup & Recovery Workflows:** Utilizes regular Proxmox snapshotting and container backups to ensure zero data loss during system updates.

---

```mermaid
graph TD
    %% Styling Classes
    classDef internet fill:#f9f,stroke:#333,stroke-width:2px;
    classDef router fill:#bbf,stroke:#333,stroke-width:2px;
    classDef server fill:#bfb,stroke:#333,stroke-width:2px;
    classDef client fill:#fbb,stroke:#333,stroke-width:2px;

    %% Nodes
    Internet["ISP / Modem"] --> USG["UniFi USG-3P Gateway"]:::router
    
    subgraph UniFi Network Infrastructure
        USG --> CK["Cloud Key Gen2 Plus"]:::router
        USG --> Switch["8-Port PoE Switch"]:::router
        Switch --> AP["Wi-Fi Access Points (Free Wi-Fi)"]:::client
    end

    subgraph Proxmox Server
        Switch --> Proxmox["Proxmox VE Hypervisor"]:::server
        
        Proxmox --> Jellyfin["LXC 101: Jellyfin"]
        Proxmox --> CatchAll["LXC 102: Catch-All Apps"]
        Proxmox --> HA["VM: Home Assistant"]
    end

    subgraph Isolated IoT Network
        AP -.-> Printer["HP LaserJet Printer"]:::client
    end
