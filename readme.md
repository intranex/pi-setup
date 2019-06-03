# pi-setup

## Hardware
* links are samples from the raspberry pi foundation
### Basic hardware list (control from pi, no screen):
1) [Raspberry Pi 3B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/)
2) [Micro USB power adapter](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/)
3) 32 GB Micro SD Card

### Touchscreen hardware list (POE powered)
1) [Raspberry Pi 3B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/)
2) [Raspberry Pi POE Hat](https://www.raspberrypi.org/products/poe-hat/)
3) [Rapsberry Pi Touch Screen](https://www.raspberrypi.org/products/raspberry-pi-touch-display/)
4) 32 GB Micro SD Card

### Hardware setup
1)	Attach the ribbon and jumper cables to the touch screen
2)	Mount the Pi on the touch screen and secure with screws
3)	Attach the POE hat to the Pi
4)	Attach the ribbon and jumper cables to the Pi (you may want to use a dab of hot glue to secure the jumper cables, but it is not necessary)


## Software
### Raspbian
1)	Download the latest raspbian lite image from raspberrypi.org
2)	Use Etcher to flash the .iso file to a SD card
 
3)	Open the SD card on your computer.  The partition drive should be called “boot”.  Create an empty text file called “ssh” (no extension) on the drive
4)	Put the SD card into the Pi and connect it to a dhcp network source with POE power
5)	SSH to the pi via the IP address on the raspberry pi screen.  The default user is “pi” and password is “raspberry”

### Other software
6)	Run these commands to get apt ready to install additional software:
`sudo apt update`
`sudo apt upgrade`
7)	Make sure that you set the date/time to the appropriate value by running this command (with the current date and time and timezone):\
`sudo timedatectl set-timezone "America/Denver"`\
`sudo date –s “Thu May 17 08:21:00 -06:00 2019”`

### i3 Window Manager
1)	Install i3 by issuing this command:\
`sudo apt install i3`

### vim
1)	Install vim by issuing this command:\
`sudo apt install vim`

### xorg xserver
1)	Install xorg xserver by issuing this command:\
`sudo apt install xserver-xorg`\
`sudo apt install xinit`

### chromium
1)	Install i3 by issuing this command:\
`sudo apt install chromium-browser`

### docker
1)	Install docker by issuing this set of commands:
2)	First, install supporting software for docker\
`sudo apt install apt-transport-https`\
`sudo apt install ca-certificates`\
`sudo apt install curl`\
`sudo apt install gnupg2`\
`sudo apt install software-properties-common`\
3)	Next, add Docker’s official GPG key:\
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`\
4)	 Next, install docker for raspbian by running these commands:\
`echo "deb [arch=armhf] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list`\
`sudo apt update`\
`sudo apt-cache policy docker-ce`\
`sudo apt install -y docker-ce=17.09.0~ce-0~debian`

5)	Set Docker to hold the version until specifically updated by running this command:\
`sudo apt-mark hold docker`

6)	Enable control of docker for the pi user by running these commands and waiting for a reboot:\
`sudo groupadd docker`\
`sudo usermod –aG docker pi`\
`sudo reboot`

7)	Start docker by running these commands\
`sudo systemctl enable docker`\
`sudo systemctl start docker`

### docker-compose
1)	Install docker compose by running these commands:\
`sudo apt install python3-pip`\
`sudo apt install libffi-dev`\
`sudo pip3 install docker-compose`

## Sample setup - Control a Sony ADCP-enabled Projector

### Setting up Docker
1) First, copy the docker-compose.yml file to the pi by running this command:\
`curl https://raw.githubusercontent.com/byuoitav/pi-setup/master/docker-compose.yml > docker-compose.yml`

2) Then, run `docker-compose up -d` to start the docker containers

3) Run `docker ps` to see the status of your docker containers

### Docker containers needed
1) couch-db
2) av-api
3) adcp-control-microservice
4) touchpanel-ui-microservice

### docker-compose.yml
[Sample docker-compose.yml file](docker-compose.yml)

### Configuration documents
Once you have the couch-db container running, you should be able to access it by going to `http://[pi-ip-addr]:5984/_utils` in a web browser, using the ip address of your pi\
You will need to create several databases and documents for a base configuration.

### Couch configuration sample documents
`buildings` database:
[BLDG](/couch-document-samples/building.json)

`device` database:
[D1](/couch-document-samples/device-D1.json)
[HDMI1](/couch-document-samples/device-HDMI1.json)
[PC1](/couch-document-samples/device-PC1.json)

`rooms` database:
[BLDG-ROOM](/couch-document-samples/room.json)

`room_configurations` database:
[Default](/couch-document-samples/room-configuration-default.json)

`device_types` database:
[ADCP Projector](/couch-document-samples/device-type-adcp-projector.json)

`ui-configuration` database:
[BLDG-ROOM](/couch-document-samples/ui-config.json)


### Controlling the room
In this scenario, we will use the AV-API to talk to a Sony ADCP projector.  The device configuration specified in the sample states that an HDMI input (HDMI1) is plugged into the HDMI port on the projector, and a PC plugged into the DVI port.sss