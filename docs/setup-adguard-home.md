#Install AdGuard Home Natively on Ubuntu VM

`I first tried the AdGuard Home in the docker but there was problem like internet was slow also it wasn't blocking the ads as expected and the speed i was getting was very much slow.Then I tried different config but didn't got the expected result. I was getting the wifi speed inly 2-3 Mbps with is way low than my actual speed(700+ with ethernet) so i tried to see if my vm and the proxmox have any issue but surprisingly the internal connection between them was 18.7 Gbits/sec...like holy shit man it's a insane..Then i tried to install it nativly in Ubuntu VM then i succeed and I got the internet speed way better like normal wifi speed i get day by day around 150-200 Mbps in daytime and in the late night after 12 AM i get around 300+ Mbps via wifi.`

`So I'd recommend to install the Adguard home nativly on ubuntu vm else if Adguard Home doesn't work for u properly u can try the Pi-hole.`

#insttalltion (Nativly)

1st remove Docker AdGuard Home if u have installed it with docker before

# Navigate to your adguard directory
cd ~/adguard

# Stop and remove the container
docker-compose down

# Remove the docker network and volumes (optional cleanup)
docker network rm adguard_default
docker volume rm adguard_data

#Install AdGuard Home Natively

# Download and run the official installer
curl -s -S -L https://raw.githubusercontent.com/AdGuardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v

# Follow the interactive prompts - choose:
# - Installation path: /opt/AdGuardHome
# - Port: 3000 for admin, 53 for DNS
# - Auto-start: Yes

 #Configure AdGuard Home

 Open setup wizard: Go to http://<ur_ubuntu_vm_ip>:3000 `give the port correctly while set it up else it wont open afterwards`

Set up:

Admin interface: 0.0.0.0:3000

DNS server: 0.0.0.0:53

Set upstream DNS: Use https://dns10.quad9.net/dns-query `I'll say about the encyption dns later in the end`

Enable filters: Add OISD + AdGuard lists `Add only OISD  with deafult adguard filter cos adding more filter will significantly increase ur query time then u may feel it's slowing down ur internet`

#Reconfigure Router

Go back to router DNS settings

Set:

Deafult Gatweay: ur_router_ip `(which u use to login into ur router's admin page`

Primary DNS: ubuntu_vm_ip `(192.168.x.x)`

Secondary DNS: 0.0.0.0 (or leave blank) `but dont set it to 1.1.1.1(cloudfare) or 8.8.8.8(google) cos ur router will then bypass ur adguard filer due to restriction and this happend for me and it literally killed a lots of time`
`(Cons of not setting a Secondary Ip in the router : If somehow ur Ubuntu_vm machine (for me my laptop) breaks ur whole internet will go down) (Pros : But wihout setting the secondary ip it will give u reliable performance over full network)`
`My  recommendation is keep the secindary dns empty`


Save and reboot the router also ur devices so that they can get the new dns .

In the adguard home admin panel go to DNS settings then set these
`https://dns10.quad9.net/dns-query
https://dns11.quad9.net/dns-query
https://security.cloudflare-dns.com/dns-query
https://1.1.1.1/dns-query`
then apply them.

Don't add more cos they can slow down ur reponse time but my recomendation would be 
`https://dns.quad9.net/dns-query
https://dns11.quad9.net/dns-query`

Cos for my these two give me better reponse time then others above also these quad dns encrypt my network better way.

At first search it might seem like its slower but thanks to dns caching and it wil become faster after the 1st lookup for a new website and u will really enjoy the speed.

But ocatinally u can a feel a little bit latency but a few ms latency is worth of that privacy. Also you will get used to it.Also ur internet speed will be same as it used to be also it will feel even faster after these steps.

After investing much  time in my homelab i came to a conclution that only ad blocer running 24/7 on the machine won't block all the ads. So its recommended to use adguard phn app also use a browser extention in ur laptop or pc for maximum protection.

`My that homelab laptop runs 24/7 and it works fine also blocks ads of my a connected devices but somehow some ads still slips so I alsp use Adguard Home app in my phn and adguard ad blocker extention in my laptop's browser .That's how doesn't pop in anywhere in my workflow. It's works super well.`



