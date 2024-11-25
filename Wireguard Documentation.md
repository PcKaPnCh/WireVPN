
## Wireguard Setup and Documentation
___
Firstly, I signed up for a new DigitalOcean account at: https://m.do.co/c/d33d59113ab6
Then, using the $200 credit, I setup the cheapest Ubuntu 24.04 Droplet that was only $6/month.
After that, I then set up the root password instead of doing ssh key and set the data center for New York.
I then followed the instructions at [this link](https://thematrix.dev/setup-wireguard-vpn-server-with-docker/) in the Droplet's console, as well as additionally installing Docker on the droplet using ```sudo apt install docker-compose```.
Firstly, use ```mkdir -p``` to create a ~/wireguard and a ~/wireguard/config directory. 
Then, use ```nano ~/wireguard/docker-compose.yml``` and copy/paste the following:

version: '3.8' \
services: \
  &emsp;wireguard: \
    &emsp;&emsp;container_name: wireguard \
    &emsp;&emsp;image: linuxserver/wireguard \
    &emsp;&emsp;environment:\
    &emsp;&emsp;&emsp;  - PUID=1000\
    &emsp;&emsp;&emsp;  - PGID=1000\
    &emsp;&emsp;&emsp;  - TZ=Asia/Hong_Kong\
    &emsp;&emsp;&emsp;  - SERVERURL=1.2.3.4\
    &emsp;&emsp;&emsp;  - SERVERPORT=51820\
    &emsp;&emsp;&emsp;  - PEERS=pc1,pc2,phone1\
    &emsp;&emsp;&emsp;  - PEERDNS=auto\
    &emsp;&emsp;&emsp;  - INTERNAL_SUBNET=10.0.0.0\
    &emsp;&emsp;ports: \
    &emsp;&emsp;&emsp;  - 51820:51820/udp\
    &emsp;&emsp;volumes: \
    &emsp;&emsp;&emsp;  - type: bind \
    &emsp;&emsp;&emsp;&emsp;    source: ./config/ \
    &emsp;&emsp;&emsp;&emsp;    target: /config/ \
    &emsp;&emsp;&emsp;  - type: bind\
    &emsp;&emsp;&emsp;&emsp;    source: /lib/modules\
    &emsp;&emsp;&emsp;&emsp;    target: /lib/modules\
    &emsp;&emsp;restart: always \
    &emsp;&emsp;cap_add: \
    &emsp;&emsp;&emsp;  - NET_ADMIN \
    &emsp;&emsp;&emsp;  - SYS_MODULE \
    &emsp;&emsp;sysctls: \
    &emsp;&emsp;&emsp;  - net.ipv4.conf.all.src_valid_mark=1 \

*make sure to change TZ to the appropriate timezone defined in this [wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones?ref=thematrix.dev) page for your location/region.*
*make sure to change the SERVERURL to the appropriate ip address for the droplet
you should only need one peer, make sure to only include one, I just used pc1 for mine*

Then, I started wireguard using by changing my location to the ~/wireguard/ directory I first created and then running ```docker-compose up -d``` 
___
In summary, the instructions just want you to create a directory for Wireguard and the configuration files as well as configuring those files a little bit and then finally using docker-compose to launch the server using the configs. 
	After following the instructions on the website and finishing the setup, I then installed the Wireguard app on my phone and then also installed the application on my laptop found at [this link](https://www.wireguard.com/install/).
	Then, I used the qr code provided by ```docker-compose logs -f wireguard``` and the Wireguard app on my phone to connect to the tunnel.
	For my laptop, I created a new .conf file in my files using the notepad application and then copied the contents of peer_pc1.conf in ~/wireguard/config/peer_pc1/ to then launch the tunnel on my laptop.
