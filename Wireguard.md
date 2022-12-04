# Introduction 
For this Wireguard Project, we are creating a Digital Ocean account and do the following:
- Create a DO Ubuntu droplet 
- Install Docker on the droplet 
- Install Wireguard 
- Test the VPN on our phone and laptop

# Creating the account and droplet 
1. For the first step, we need first create an account on DigitalOcean, since Codi provided a link on the powerpoint slides, click it. This will give us $200 credit for 2 month of usage: `https://m.do.co/c/4d7f4ff9cfe4` . Sign up by inputting your email and password. 
2. For this step, we need to create an Ubuntu 20.04 Droplet, first by creating a new project and have these as the following: `Ubuntu 20.04, Basic, and Regular Intel CPU`. We want the cheapest droplet so do $4/month. For the datacenter, you can send it to whatever you want (I chose New York). Below image is what you should have it as.

![Screenshot 2022-12-03 183001](https://user-images.githubusercontent.com/87620828/205471361-c01ed323-fa9a-402b-a3f4-e1c1520e9501.jpg)


4. After having those setting, there will be a section if you want to choose either SSH Key or Password. While password is much easier to do, it is less secure than SSH Key. For this installation process, I decided to go with SSH. Once you click `Add public SSH key`, a new window will pop up where it will ask to paste your public key. On the right side of the window, there is  a tutorial where it will tell you how to do it. Follow the steps and copy and paste your public key. After doing that, name your SSH Key and click `Add SSH Key`. Below is the image of the window.

![Screenshot 2022-12-03 183235](https://user-images.githubusercontent.com/87620828/205471486-aca0ae39-3944-4de1-809e-e0c9847fdadc.jpg)

6. Once you complete creating your SSH Key, create your droplet by scrolling down and create droplet. We do not need to choose any optional addons so ignore the remaining section. 

# Install Docker
1. For this step, we need to install Docker onto the Ubuntu that we just created. Since Codi provided us instructions to install both the Docker and Wireguard, we will be using these instructions to walk us through both installing the Docker and Wireguard. Firstly, click where it says console after your ubuntu is installed. 
2. Type `sudo



