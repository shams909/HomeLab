#Docker and Docker-compose installation

#install Docker

ssh into your ubuntu-vm then run these commands

# Update the package list
sudo apt update && sudo apt upgrade -y

# Install required packages for apt over HTTPS
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable Docker repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y

# Add your user to the 'docker' group to run commands without sudo
sudo usermod -aG docker $USER

# Install Docker-Compose
sudo apt install docker-compose -y

# Activate the group change (or just log out and back in)
newgrp docker

##Verify that it worked

`docker --version
docker-compose --version
sudo systemctl status docker`

`If you see version numbers and an "active (running)" status, you've successfully built your Docker host!`

`For me the those commands didn't work due to some previous installation files and the system got kind of messed up. Then I used these commands below and those ran flawlessly` 

##For Docker
# Download the official Docker installation script
curl -fsSL https://get.docker.com -o get-docker.sh

# Make the script executable
chmod +x get-docker.sh

# Run the installation script with sudo
sudo sh get-docker.sh

Wait for the installation to complete. This might take a few minutes. It took less than a minute for me.

After the installation finishes, run these commands:

# Add your user to the docker group
sudo usermod -aG docker $USER

# Enable Docker to start on boot
sudo systemctl enable docker

# Start Docker service
sudo systemctl start docker

# Apply group changes (you might need to logout/login later)
newgrp docker

Now verify that Docker is working:

# Check Docker version
docker --version

# Check Docker service status
sudo systemctl status docker

# Test with hello-world
docker run hello-world

`Yup now docker is running properly`

#Now for docker-compose installation

# Download the latest docker-compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make it executable
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker-compose --version

Finally Docker and Docker-compose is running properly.
