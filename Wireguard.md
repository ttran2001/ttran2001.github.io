# Introduction 
For this Wireguard Project, we are creating a Digital Ocean account and do the following:
- Create a DO Ubuntu droplet 
- Install Docker on the droplet 
- Install Wireguard 
- Test the VPN on our mobile device and laptop

# Creating the account and droplet 
1. For the first step, we need first create an account on DigitalOcean, since Codi provided a link on the powerpoint slides, click it. This will give us $200 credit for 2 month of usage: `https://m.do.co/c/4d7f4ff9cfe4` . Sign up by inputting your email and password. 
2. For this step, we need to create an Ubuntu 20.04 Droplet, first by creating a new project and have these as the following: `Ubuntu 20.04, Basic, and Regular Intel CPU`. We want the cheapest droplet so do $4/month. For the datacenter, you can send it to whatever you want (I chose New York). Below image is what you should have it as.

![Screenshot 2022-12-03 183001](https://user-images.githubusercontent.com/87620828/205471361-c01ed323-fa9a-402b-a3f4-e1c1520e9501.jpg)


4. After having those setting, there will be a section if you want to choose either SSH Key or Password. While password is much easier to do, it is less secure than SSH Key. For this installation process, I decided to go with SSH. Once you click `Add public SSH key`, a new window will pop up where it will ask to paste your public key. On the right side of the window, there is  a tutorial where it will tell you how to do it. Follow the steps and copy and paste your public key. After doing that, name your SSH Key and click `Add SSH Key`. Below is the image of the window.

![Screenshot 2022-12-03 183235](https://user-images.githubusercontent.com/87620828/205471486-aca0ae39-3944-4de1-809e-e0c9847fdadc.jpg)

6. Once you complete creating your SSH Key, create your droplet by scrolling down and create droplet. We do not need to choose any optional addons so ignore the remaining section. 

# Install Docker
1. For this step, we need to install Docker onto the Ubuntu that we just created. Since Codi provided us instructions to install both the Docker and Wireguard, we will be using these instructions to walk us through both installing the Docker and Wireguard. Firstly, click where it says console after your ubuntu is installed. 
2. Type `sudo apt install apt-transport-https ca-certificates curl software-properties-common -y`. This will install the necessary tools to install Docker
3. Type `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`. This will add the Docker Key.
4. Add the Docker repo by typing 
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
5. After you add the Docker repo, switch to the repo you just install by typing `apt-cache policy docker-ce` 
6. Install Docker by typing `sudo apt install docker-ce -y`. Once installed `sudo usermod -aG docker ${USER}` to allow to use Docker without sudo just in case if you are not in the root.  
7. Log out and log back in by exiting the console and opening the console. 
8. Type `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`. This will install the Docker-Compose. 
9. Type `sudo chmod +x /usr/local/bin/docker-compose` to set the permission for Docker-Compose. 

# Installing Wireguard
1. Once you complete installing the Docker, it now time to install Wireguard. Start by typing these onto your server 
```
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```  

This will create the directory for `wireguard`, `/wireguard/config` and edit the `wireguard/docker-compose.yml`
2. Copy and paste, from the instructions that Codi provided us, below: 
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```
3. Modify the following places: 
- `TZ` to the timezone of your choosing 
- `SERVERURL` to your ipv4 address that is provided in your DigitalOcean dashboard 
- `PEERS` if you wanna modify the number of user-config files that you want it to generate. Leave it as it is 
After doing that, save it by  `CTRL` + `X`, `Y`, and click `ENTER` to both save and exit the file.  
4. Lastly, start Wireguard by typing `cd ~/wireguard/`, `ENTER`, `docker-compose up -d`, and `ENTER`. This will build the server. You will know it is done once it says `Creating wireguard   ... done`

# Testing the VPN on our mobile device 
1. For this step, we need to know if our VPN is working on our mobile device. To start, first install the Wireguard application on your mobile device. Type `ipleak.net` on your mobile device browser and take a screenshot. This will show the local IP info before we turn on the VPN

3. Once it install, go back to your console and type the following: `docker-compose logs -f wireguard`. This will generate three QR codes for `pc1`, `pc2`, and `phone1`. Scan `phone1` by opening your Wireguard VPN application, click the `+` button, and click `Create from QR code`. Scan the QR code. Follow the following steps and turn on the VPN on and repeat the second to last sentence of the last step. 






