# Ubuntu VM Setup (Proxmox)

## 1. Download ISO

- Go to [Ubuntu Server Downloads](https://ubuntu.com/download/server)

- Choose **Ubuntu Server LTS (recommended)** (e.g., 24.04 LTS)

- Upload ISO to Proxmox: Datacenter -> Storage -> ISO Images -> Upload or Use the option to use url to download (recommended)

## 2. Create a New VM in Proxmox

- Go to **Proxmox Web UI → Create VM**

- Name: `ubuntu-server`

- OS: Select the uploaded ISO

- System:

  - BIOS: `OVMF` (UEFI) (if supported)

  - Machine: `q35`

- Disks:

  - Size: `75GB` (or more depending on usage)

  - Storage: `local-lvm`

- CPU: `2 Cores` (adjust as needed)

- Memory: `8042MB` (less or more)

- Network: `VirtIO (paravirtualized)`

## 3. Install Ubuntu

- Boot VM from ISO

- Follow installer prompts:

  - Language: English

  - Install Ubuntu Server

  - Configure user + password

  - Configure SSH (enable OpenSSH server during setup)

  - Skip optional packages for now

- Finish installation and reboot

## 4. Post-Installation Setup

# Update system

sudo apt update && sudo apt upgrade -y

# Install basic tools

sudo apt install curl wget git vim net-tools htop -y

5. Enable Remote Access `recommended`

Find IP:

`ip a`

Connect via SSH in terminal:
`ssh user@<VM-IP>`

Reboot

voila! Ur ubuntu vm setup is done..now u can access it via ssh.
