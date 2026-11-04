# Home Assistant Virtual Machine (VM) Architecture

## 🏠 Overview
Home Assistant serves as the central orchestration engine for local home automation, device telemetry, and sensor monitoring. To ensure strict process isolation, resource stability, and hardware passthrough capabilities, it is deployed as a dedicated **Virtual Machine (VM)** running on the Proxmox VE hypervisor rather than a shared container.

---

## ⚙️ Proxmox VM Resource Allocation
To maintain a responsive automation loop while conserving system resources across the Proxmox node, the Home Assistant VM is provisioned with balanced virtual hardware specifications:
* **Hypervisor Environment:** Hosted on Proxmox VE (Dell Optiplex hardware node).
* **Virtual Disk Storage:** Assigned directly to the **2TB secondary internal SSD** tier to eliminate input/output (I/O) bottlenecks and protect hypervisor boot volumes from continuous database write wear.
* **Core & Memory Allocation:** Configured with dedicated vCPUs and RAM to support real-time event processing without competing with media streaming services.

---

## 🔌 Integration & Network Segmentation
Home Assistant relies on local-first communication protocols to ensure zero cloud dependency and high-speed local control:
* **Network Isolation:** Operates alongside the primary trusted network ("Free Wi-Fi") while maintaining secure API communication channels with isolated IoT devices and smart plugs.
* **Local Integrations:** Directly interfaces with local hardware nodes, automation routines, and webhook triggers.

---

## 🛡️ Backup & Disaster Recovery Protocol
Because home automation states and historical automation triggers change frequently, automated data protection is essential:
* **Proxmox Snapshots & Backups:** Scheduled hypervisor-level backups capture the entire virtual machine state, ensuring rapid recovery in the event of a system update failure or host migration.
* **Internal Application Backups:** Automated snapshot tarballs are exported regularly to prevent configuration or automation loss.
