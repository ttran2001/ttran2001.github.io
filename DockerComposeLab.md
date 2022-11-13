# Docker Compose Lab (Project 2) 

## Introduction 
For this project, the objective is to install Docker Environment and have the choice into three main choices of either:
- Installing OpenVAS via Docker
- Installing WordPress via Docker 
- Installing PiHole via Docker

For my choice, I decided on installing `Docker Environment` via Windows and went with the option of installing `WordPress via Docker`. These are the steps I took into installing both Docker and WordPress via Docker. 

## Installing Docker Enviroment
For the first step, we need to install the Docker Enviroment before we can install our WordPress. For this step, this step was simple and easy to install.

1. Go onto the Docker website and install the Docker of your choosing. I decided to go with Windows for this installation. 

![0](https://user-images.githubusercontent.com/87620828/201506625-0e673096-1d25-43ab-99db-a68c0be431eb.jpg)

2. After the download is finish, click the accept button when te Service Agreement window pop up.
3. Since the Docker needs privilege, we need to go into our powershell and install a file to make our Docker work. 



## Installing WordPress via Docker 

Congrats, you just installed Docker! For the next step, we are going to install `WordPress via Docker`. This process is both easy and fast. Did not have any problems doing this step. 

1. Open your command prompt and type the following: 
- `docker compose --version` : Confirms that we downloaded the updated version 
- `mkdir WordPress` : creates a new directory call `WordPress` 
2. After creating your WordPress director, go to the File Explorer and to where you placed your `WordPress" Directory. For me, it was "This PC > Windows-SSD (C:) > Users > tt553 > WordPress`
3. Create a new .yml file of your choosing and call it `docker-compose.yml` use the following code. I used NotePad++. After that, use the following code in the bottom and save 
![0](https://user-images.githubusercontent.com/87620828/201506906-f6dc90fe-d02c-427b-bb74-e34f08da2ec9.jpg)
4. Go back to your command prompt and type the following:
- `cd WordPress`: Navigates to the `WordPress` directory
- `docker compose up -d`: Creates the containers by running the code that you put for `docker-compose.yml` file. 

This will take a couple of minutes to install the file. 
5. After installing the file, open your browser and type http://localhost:8000/. The reason we put 8000 is because this is what we set to our port in our `docker-compose.yml` file. If this screen below pops up, this means you installed WordPress successfully 
![0](https://user-images.githubusercontent.com/87620828/201506992-2daf677e-b041-4af0-9eb0-a87518f3d235.jpg)
6. Select the best language to you and click continue 
7. Fill in the Site Title, Username, Password and Your Email. After that, click the `Install WordPress` button

## Conclusion

Congrats, if you get this page below, this means that you completed the Docker Compose Lab. Below is the Admin Interface and at the top right of the site is my username. Overall, this project was easy to do since I went with the WordPress route. Did not have any trouble or any problems I had in this lab. 

![0](https://user-images.githubusercontent.com/87620828/201507067-29184247-dcd1-4c71-8195-597dc4210ca5.jpg)
