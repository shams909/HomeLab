#Homarr Installation

`Create a config folder`
mkdir -p ~/homarr/configs
mkdir -p ~/homarr/icons


`Pull the latest image of homarr`
docker pull ghcr.io/ajnart/homarr:latest

`Run it`

docker run -d \
  --name homarr \
  -p 7575:7575 \
  -v ~/homarr/configs:/app/data/configs \
  -v ~/homarr/icons:/app/public/icons \
  ghcr.io/ajnart/homarr:latest

`Now check the browser`

http://<your-server-ip>:7575

`Now edit according to ur preferences`

🚀 One-Line Update for Homarr

Whenever Homarr notifies me of an update, I run this in my ~/homarr folder:

docker compose pull && docker compose up -d

##My messy little homarr dashboard 🥹
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/de7d55f1-ee68-42c9-a326-8dbd54f1060d" />


