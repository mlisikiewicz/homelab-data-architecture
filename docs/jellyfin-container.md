# Jellyfin Media Server Container Architecture

## 🎬 Overview
Jellyfin serves as the primary self-hosted media streaming platform, providing centralized access to movies, television series, and audio libraries. To ensure strict resource isolation, maintain high streaming performance, and protect hypervisor boot volumes from continuous read/write wear, Jellyfin is deployed as an isolated Linux Container (LXC) on the Proxmox VE hypervisor.

---

## 💾 Storage & Data Persistence Strategy
Media streaming and metadata indexing generate heavy input/output (I/O) traffic. To protect system hardware, storage operations are strictly tiered:
* **Boot / System Drive Isolation:** The lightweight Proxmox hypervisor boot drive is strictly isolated from media workloads to prevent write fatigue.
* **Secondary Storage Tier:** All application configuration data, user metadata databases, and bulk media libraries are mapped directly to the **2TB internal SSD** (`/mnt/pve/ssd2tb/`), ensuring high-speed file retrieval and zero bottlenecks during playback.

---

## 🚢 Container Deployment & Resource Allocation
* **Container Type:** Hosted in a dedicated Proxmox Linux Container (LXC) to maximize hardware efficiency while keeping streaming services completely separate from background utility applications.
* **Volume Mounts:** Uses explicit bind mounts linking directly to the high-performance SSD storage pools for fast thumbnail generation, artwork caching, and stream buffering.

---

## 🔄 Maintenance & Backup Protocols
* **Database Preservation:** Metadata caches and user watch-state databases are backed up regularly alongside hypervisor-level snapshot routines.
* **Stream Optimization:** Configured to handle direct-play and hardware-accelerated transcoding where supported, minimizing CPU overhead on the hosting node.
