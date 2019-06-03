# pi-setup

## Software
### Raspbian
1)	Download the latest raspbian lite image from raspberrypi.org
2)	Use Etcher to flash the .iso file to a SD card
 
3)	Open the SD card on your computer.  The partition drive should be called “boot”.  Create an empty text file called “ssh” (no extension) on the drive
4)	Put the SD card into the Pi and connect it to a dhcp network source with POE power
5)	SSH to the pi via the IP address on the raspberry pi screen.  The default user is “pi” and password is “raspberry”
6)	Run these commands to get apt ready to install additional software:
`sudo apt update`
`sudo apt upgrade`
7)	Make sure that you set the date/time to the appropriate value by running this command (with the current date and time and timezone):
`sudo timedatectl set-timezone "America/Denver"`
`sudo date –s “Thu May 17 08:21:00 -06:00 2019”`

### i3 Window Manager
1)	Install i3 by issuing this command:
`sudo apt install i3`

### vim
1)	Install vim by issuing this command
`sudo apt install vim`

### xorg xserver
1)	Install xorg xserver by issuing this command:
`sudo apt install xserver-xorg`
`sudo apt install xinit`

### chromium
1)	Install i3 by issuing this command:
`sudo apt install chromium-browser`

### docker
1)	Install docker by issuing this set of commands:
2)	First, install supporting software for docker
`sudo apt install apt-transport-https`
`sudo apt install ca-certificates`
`sudo apt install curl`
`sudo apt install gnupg2`
`sudo apt install software-properties-common`
3)	Next, add Docker’s official GPG key:
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
4)	 Next, install docker for raspbian by running these commands
`echo "deb [arch=armhf] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list`

`sudo apt update`
`sudo apt-cache policy docker-ce`
`sudo apt install -y docker-ce=17.09.0~ce-0~debian`

5)	Set Docker to hold the version until specifically updated by running this command:
`sudo apt-mark hold docker`

6)	Enable control of docker for the pi user by running these commands and waiting for a reboot:
`sudo groupadd docker`
`sudo usermod –aG docker pi`
`sudo reboot`

7)	Start docker by running these commands
`sudo systemctl enable docker`
`sudo systemctl start docker`

### docker-compose
1)	Install docker compose by running these commands:
`sudo apt install python3-pip`
`sudo apt install libffi-dev`
`sudo pip3 install docker-compose`

## Base setup - Control an ADCP Projector
### Containers needed
1) couch-db
2) av-api
3) adcp-control-microservice
4) touchpanel-ui-microservice

### docker-compose.yml

