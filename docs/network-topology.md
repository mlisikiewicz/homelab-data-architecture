# UniFi Network Topology & Security Architecture

## 🌐 Overview
A secure, segmented network architecture is essential for isolating core server infrastructure from background guest and IoT traffic. This network is managed entirely through a UniFi gateway and switching ecosystem, enforcing strict boundary controls and local DNS filtering.

---

## 🛠️ Hardware Stack & Physical Topology
* **Gateway:** UniFi USG-3P Router handling routing, firewall rules, and stateful packet inspection.
* **Controller:** UniFi Cloud Key Gen2 Plus managing network configuration, client monitoring, and UniFi OS operations.
* **Switching:** 8-Port PoE Switch providing high-throughput wired connectivity to access points and hypervisor nodes.
* **Access Points:** UniFi Wi-Fi access points broadcasting segregated wireless networks.

---

## 🔒 Logical Network Segmentation & VLANs
To prevent unauthorized access to sensitive server assets, endpoints are separated into distinct virtual local area networks (VLANs):
* **Primary Network ("Free Wi-Fi"):** Dedicated trusted wireless network for primary workstations, personal devices, and administrative tasks.
* **Isolated IoT Network:** Quarantined VLAN restricted from initiating inbound traffic to the primary network or server shares. Hosts smart home devices, miscellaneous sensors, and network printers (e.g., HP LaserJet).
* **Kids' Network ("Mini-WiFi"):** Dedicated isolated wireless network for household media and personal devices, keeping traffic cleanly separated from core administrative paths.

---

## 🛡️ Security & Firewall Rules
* **Inter-VLAN Blocking:** Firewall rules are implemented at the gateway level to block IoT and guest segments from communicating with administrative management interfaces.
* **Local DNS & Traffic Management:** Centralized local network-wide filtering optimizes bandwidth and secures internal endpoint communication.
