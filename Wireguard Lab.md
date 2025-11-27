## Preparation
1. Created a DigitalOcean account
2. Started Ubuntu droplet
## PowerShell
1. SSH'd into the droplet using the root account
2. Followed [Docker install instructions](https://docs.docker.com/engine/install/ubuntu/)
	- created a clark user for myself so that I'm not using root
	- Uninstalled old versions of docker packages with `sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)`
	- No packages found
	- Set up Docker apt repository
	- Installed Docker with `sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
3. Edited a docker compose file from [here](https://github.com/linuxserver/docker-wireguard) - this is my docker-compose.yaml:
	```dockercompose.yaml
	services:
	  wireguard:
	    image: lscr.io/linuxserver/wireguard:latest
	    container_name: wireguard
	    cap_add:
	      - NET_ADMIN
	    environment:
	      - PUID=1000
	      - PGID=1000
	      - TZ=America/Chicago
	      - SERVERURL=137.184.38.238
	      - SERVERPORT=51820
	      - PEERS=2
	      - PEERDNS=auto
	      - LOG_CONFS=true
	      - INTERNAL_SUBNET=10.13.13.0
	    volumes:
	      - /home/clark/wireguard/config:/config
	      - /lib/modules:/lib/modules
	    ports:
	      - 51820:51820/udp
	    sysctls:
	      - net.ipv4.conf.all.src_valid_mark=1
	    restart: unless-stopped
	```
4. Started the container with `docker compose up`
5. Using the QR code I connected on my phone:
![unchanged ip](https://github.com/cm-butler/cm-butler.github.io/blob/main/WireImages/SS1.PNG?raw=true)
![changed ip](https://github.com/cm-butler/cm-butler.github.io/blob/main/WireImages/SS2.PNG?raw=true)

6. I then added the profile to my computer
![unchanged laptop ip](https://github.com/cm-butler/cm-butler.github.io/blob/main/WireImages/SS3.PNG?raw=true)
![changed laptop ip](https://github.com/cm-butler/cm-butler.github.io/blob/main/WireImages/SS4.PNG?raw=true)