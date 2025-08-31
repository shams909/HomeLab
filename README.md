# HomeLab
##🏡 Homelab Journey – My Mini Data Center

###Turning my laptop into a self-hosted mini data center.
###Documenting my full homelab journey with Proxmox, Docker, dashboards, AdGuard, Tailscale, and automation scripts.

🔥 About This Project

This repo is my logbook + portfolio for my homelab journey.
I started with an old laptop and grew it into a mini data center with virtualization, automation, networking, and monitoring. Everything is open, documented, and evolving.

💻 Hardware Setup

Component	Details
💻 Laptop	HP Pavilion – Intel i7 7th Gen U-series, Nvidia 940MX
💾 Memory	20GB RAM (4GB soldered + 16GB stick)
🔧 Storage	SATA SSD (previously had 1TB HDD, RIP 🥹)
📡 Router	TP-Link Archer C6 V4
🖧 Network	Cat6 Ethernet (700+ Mbps vs <100 Mbps on Cat5e 💀)
⚙️ Core Software Stack

🖥️ Proxmox VE → virtualization & VM management

🐧 Ubuntu Server VMs → running most services

🐳 Docker + Portainer → containerized apps & management

🚫 AdGuard Home → DNS ad-blocking (natively installed on Ubuntu VM)

🔐 Tailscale VPN → zero-trust remote access

🤖 Automation Scripts → health checks & Telegram bot updates

🛠️ Services Running

Portainer – manage Docker containers

AdGuard Home – DNS-based ad-blocking

Homarr + Dashy – dashboards for everything in one place

Telegram Bot – system health monitoring & alerts

📊 Monitoring & Automation

✅ Daily health-check scripts (CPU, RAM, disk, services)
✅ Telegram notifications every 3 hours
🔜 Grafana + Prometheus dashboards (planned)

🔐 Networking & Security

🔒 Tailscale VPN → private, secure access

🌍 Local DNS routing with AdGuard

🛡️ Firewall + IDS/IPS coming soon

🗺️ Roadmap

 Proxmox VE setup

 Docker + Portainer

 AdGuard Home + DNS routing (encrypted)

 Tailscale VPN

 Grafana + Prometheus monitoring (in future)

 Automated update system via telegram bot

 Kubernetes/Nomad for orchestration(in future)

 Trading Bot + Personal cloud with media server + personal music server( in future )

 High Availability (future dream 🚀)

🤝 Why I’m Sharing

This repo is my knowledge base + inspiration log.
I believe homelabs are the best way to learn real-world DevOps, networking, and automation.

⚡ Stay tuned — this homelab is evolving into a self-hosted data center!
