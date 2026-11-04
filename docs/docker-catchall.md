# Catch-All Application Container & Docker Architecture

## 🐳 Overview
To optimize resource utilization and avoid the overhead of running a separate virtual machine or container for every single lightweight service, this architecture utilizes a centralized "catch-all" Linux Container (LXC) running **Docker and Docker Compose**. 

This environment hosts various supporting applications, automation tools, and media indexers while maintaining strict resource boundaries and data persistence.

---

## 💾 Storage & Data Persistence Strategy
To prevent excessive write wear on the primary hypervisor boot drive, all persistent state data, configuration files, and application databases are strictly isolated:
* **Mount Path:** Mapped directly to the high-performance **2TB secondary internal SSD** (`/mnt/pve/ssd2tb/appdata/`).
* **Volume Isolation:** Each container service uses explicit bind mounts pointing to dedicated subdirectories, ensuring that container updates or reinstalls never risk user configuration data or logs.

---

## 🛠️ Core Services Hosted
The catch-all container orchestrates several independent services via Docker Compose:
* **Media Automation & Indexing:** Manages automated requests, metadata organization, and indexer coordination for home media pipelines.
* **Download Clients:** Handles secure background transfers with isolated storage directories to prevent incomplete files from cluttering production paths.
* **Utility & Management Dashboards:** Provides lightweight monitoring and web management interfaces for local infrastructure control.

---

## 📄 Sample Docker Compose Architecture
Below is a structural overview of how services are deployed and bound to persistent storage on the secondary SSD:

```yaml
version: "3.8"

services:
  app-example:
    image: linuxserver/baseimage
    container_name: catchall-service
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /mnt/pve/ssd2tb/appdata/service-name:/config
      - /mnt/pve/ssd2tb/media:/data/media
    ports:
      - "8080:8080"
    restart: unless-stopped
