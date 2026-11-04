# Data Pipelines & Storage Architecture

## 📊 Overview
In any robust data or supply chain environment, protecting system integrity while managing high-throughput data flows is critical. This infrastructure is architected to eliminate single points of failure, optimize resource utilization, and protect delicate boot volumes from excessive read/write fatigue.

---

## 💾 Storage Tiering & Hardware Isolation
To ensure high availability and prevent performance bottlenecks, storage operations are strictly divided into two distinct tiers:
* **Boot & Hypervisor Tier (128GB USB Thumb Drive):** Isolated strictly to low-write hypervisor boot operations. No heavy logging, application data, or media files are written to this volume, preventing premature flash memory wear.
* **High-Performance Data Tier (2TB Internal SSD):** All heavy input/output (I/O) workloads—including virtual machine disks, container appdata, relational databases, system logs, and media streaming files—are routed directly to a dedicated secondary internal solid-state drive (`/mnt/pve/ssd2tb/`).

---

## 🔄 Data Ingestion & Lifecycle Management
Just like managing inventory stock flow in a supply chain, data must be ingested, structured, and organized cleanly before downstream consumption:
* **Automated Indexing & Enrichment:** Incoming media and document assets are processed through automated file-staging directories, enforcing strict folder hierarchies and naming conventions.
* **Metadata Normalization:** Resolves ID mismatches and organizes relational metadata so that client applications (like Jellyfin and Audiobookshelf) can instantly index and serve assets without manual intervention.

---

## 🛡️ Backup, Recovery & Disaster Planning
* **Proxmox Snapshots:** Scheduled hypervisor-level snapshots capture virtual machine and container states, allowing for rapid rollbacks and zero data loss during system updates.
* **Persistent Bind Mounts:** Container configuration files are separated from container binaries using dedicated bind mounts on the 2TB SSD, ensuring that updating or rebuilding a container never risks data loss.
