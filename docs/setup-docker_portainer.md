# Portainer Installation

Portainer is a management interface for containerized environments, primarily **Docker** and **Kubernetes**.  
It provides a **user-friendly GUI** to simplify deployment, management, and monitoring of containers, images, networks, volumes, and other resources.

## Why use Portainer

Using Portainer is **highly recommended** because it makes Docker management easier, especially for beginners or for quickly visualizing containers and volumes.

## Installation

> **Note:** I used `cat` to create the `docker-compose.yml` file because in Arch Linux, the Kitty terminal sometimes doesn’t handle YAML formatting well. You can also use `vim` or another editor if you prefer.

### Step 1: Create `docker-compose.yml`

cat > docker-compose.yml << 'EOF'

version: '3.8'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    ports:
      - "8000:8000"
      - "9443:9443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./portainer_data:/data
EOF

### Step 2:Verify the file

cat docker-compose.yml

### Step 3:Run Portainer

docker-compose up -d

### Step 4: Access Portainer

Open your browser and go to:

https://<YOUR_UBUNTU_VM_IP>:9443


⚠️ Caution

9443 is the secure HTTPS port we mapped for Portainer.

Always use HTTPS, not HTTP, to access Portainer.
