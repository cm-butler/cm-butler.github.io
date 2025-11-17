#### I am using a Fedora GNOME VM for this assignment
1. First I made sure an old version of docker wasn't installed with the `dnf remove` command provided in the installation guide
	- Nothing was uninstalled
2. Added docker repository to dnf and installed docker with `sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
3. Enabled docker with `systemctl`
4. I decided to install a [minecraft server docker image](https://github.com/itzg/docker-minecraft-server?tab=readme-ov-file) and got the docker compose from [here](https://docker-minecraft-server.readthedocs.io/en/latest/)
	- this is the docker compose that it gave me:
	- ```
	  services:
		  mc:
		    image: itzg/minecraft-server:latest
		    pull_policy: daily
		    tty: true
		    stdin_open: true
		    ports:
		      - "25565:25565"
		    environment:
		      EULA: "TRUE"
		    volumes:
		      - ./data:/data
	  ```
5. ran `sudo docker compose up`
![first docker screenshot](/DockerImages/DockerSS1.png)
6. Shutdown VM and port forwarded the NAT network adapter so i could connect to it on my host OS
![second docker screenshot](/DockerImages/DockerSS2.png)
