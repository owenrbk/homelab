# Homelab
Summary: 
- A 3-node Proxmox homelab on a 10.0.1.0/24 LAN. The M900 handles compute, storage, and Minecraft hosting. The M93p serves as the network router (OPNsense), firewall, and DNS. The Intel NUC manages critical infrastructure, including backups, system monitoring, and UPS power management. All nodes are securely accessible via WireGuard.

Homelab System Manifest (v1.0)
Architectural Overview

A three-node Proxmox-based environment segmenting core compute, network infrastructure, and dedicated backup, operating on a 10.0.1.0/24 internal LAN nested behind a primary 192.168.1.0/24 gateway.

**Primary Compute Node: M900 (Lenovo ThinkCentre Tiny)**
- Role: Heavy Lifting & Media.
- Key Specs: i5-6500T, 16GB DDR4, 1TB NVMe, MediaSonic 4-Bay 2TB DAS.
- Critical Services:
  - Immich: 750gb vdisk on SSD to store gallery, 1TB HDD rsync backup
  - Samba/NFS shares: On 500GB HDD with secondary 500GB HDD rsync backup
  - Game Hosting: Minecraft Server managed with Crafty Controller and hosted via playit.gg

**Infrastructure Node: M93p (Lenovo ThinkCentre Tiny)**
- Role: Network Router/DNS/Firewall
- Key Specs: i5-4570T, 8GB DDR3, 128GB SATA, Intel NIC (Router on a stick)
- Critical Services:
  - Networking: OPNsense VM (acting as the 10.0.1.1 gateway) and WireGuard VPN (remote access) with Unbound.
  - DNS: Pi-hole works with Unbound for recursive, filtered DNS.

**Backup Node: NUC (Intel DC3217IYE)**
- Role: Server Backup and Monitoring Tools.
- Key Specs: i3-3217U, 6GB DDR3, 120GB mSATA, 320GB DAS.
- Critical Services:
  - Backup Server: Deduplicated and encrypted backups stored locally on the mSATA, then offloaded to a 320GB drive in the DAS for long-term retention.
  - Homepage: Organizes all my services on one page
  - Monitoring: Prometheus, Grafana, and Uptime Kuma monitoring.
  - Power: Centralized NUT (Network UPS Tools) server managing an APC BE600M1.

Network Logic & Security
Double NAT Management: OPNsense WAN has "Block private networks" disabled to permit traffic from the 192.168.1.0/24 upstream subnet.
Remote Access: Secured via WireGuard; direct local access to GUIs permitted on Port 443 for the home network.

Future Possible Projects
  M900: Media Stack: Jellyfin (Quick Sync passthrough), Arr Stack, and qBittorrent.
  M93p: NGINX Proxy Manager, CrowdSec, Network Status Bots
  NUC: Home Assistant


!(Network Diagram)[images/homelab-diagram.png]
