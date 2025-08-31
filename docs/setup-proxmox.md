🚀 Proxmox VE Setup Guide

This guide documents how I set up Proxmox VE on my homelab server.
Proxmox is the backbone of my mini data center — I use it to virtualize workloads, run Ubuntu Server VMs, and manage Docker containers inside those VMs.

🖥️ 1. Installation

Download Proxmox ISO

Get the latest ISO from Proxmox Downloads

https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso

Create Bootable USB

# Example (Linux)
sudo dd if=proxmox-ve.iso of=/dev/sdX bs=4M status=progress
sync

## I used a GUI app in Arch linux and its working perfectly fine else in windows u can use sofware like RUFUS (recommended)

Boot & Install

Boot the server from the USB.

Select Install Proxmox VE.

Accept license.

Choose target SSD for install.

Set root password and email.( use ur own email for critical updates)

Configure network (static IP recommended).

🌐 2. Accessing Proxmox Web UI

After reboot, access from browser:

https://<your-server-ip>:8006 ( ur ip will be displayed after the first boot )


Example:

https://192.168.0.100:8006


Login with:

User: root

Password: (set during install)

Realm: PAM

⚠️ Browser may warn "Not Secure" → that’s normal (self-signed cert). then go to advanced the go for the site and you will be fine.

🛠️ 3. Initial Configuration

Update Proxmox
apt update && apt full-upgrade -y

Then go for Ubuntu VM installation.
