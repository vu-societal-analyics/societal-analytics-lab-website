---
title: "Install AmCAT in your own server"
date: 2024-11-18T12:33:46+10:00
---

Written by Jana Bernhard-Harrer and Sofia Gil-Clavel.

-   [Index:](#install-amcat-in-your-own-server)
    -   [Step 1: Connect to your server and log in](#step-1-connect-to-your-server-and-log-in)
    -   [Step 2: Install Docker](#step-2-install-docker)
    -   [Step 3: Install AmCAT](#step-3-install-amcat)
    -   [Extra: In case the Docker installation was not succesful](#extra-in-case-the-docker-installation-was-not-succesful)


# Install AmCAT in your own server

## Step 1: Connect to your server and log in

`ssh <username>@<serveraddress>`

## Step 2: Install Docker (we will use this to install AmCAT)

1. Check which Ubuntu version you have to know which Docker you need: `cat /etc/os-release` 
2. Go to: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository and follow the instructions.

	1. Set up Docker's apt repository.
	  *Side Note:* `apt` installs linux system packages the same way `conda install` installs python packages.
	2. Install the latest Docker packages.
	3. Check if the Docker is running with: 
	  `sudo systemctl status docker` 
	  *Side Note:* sudo tells the server that you have admin rights. 
	4. Verify that the installation is successful 
	3. In case you need to delete a Docker you need to follow the steps in [Extra: In case the Docker installation was not succesful](#extra-in-case-the-docker-installation-was-not-succesful).

## Step 3: Install AmCAT

1. go to: https://github.com/ccs-amsterdam/amcat4docker/blob/caddy/README.md and follow the instructions: 
2. Clone the git repository with `git clone https://github.com/ccs-amsterdam/amcat4docker` 
3. and then move into the newly created folder: `cd amcat4docker`
4. launch (read: start) the docker with `sudo docker compose up -d` this might take a bit, but after you can check
5. with `curl localhost/amcat` if it worked. The output for this command should be an html code and if you go to your browser and type http://localhost it should show AmCAT
6. to configure AmCAT to your needs you need to modify the .env file.
	1. navigate to your amcat4docker folder and  `cp .env.example .env` to copy the example environment they provided
	2. run `nano .env` to be able to write into the file. 
	3. look at the example environment here: https://github.com/ccs-amsterdam/amcat4docker/blob/caddy/.env.example and change all the things that need changing. 
		1. this means: look at all the commands that are un-commented. They need input from you! 
		2. then go though all the commented settings and check whether the default settings are ok. 
			1. if the default settings are ok, leave them commented-out
			2. if you change them, you need to un-comment them.
7. check in your browser if anything works! 

10. you are done :) 

## Extra: In case the Docker installation was not succesful

1. first get the names / ids of all the dockers with `sudo docker ps`
2. then stop the dockers: `sudo docker stop -nameofdocker-`
3. then remove the containers: `sudo docker rm -nameofdocker-`
4. then prune everything: `sudo docker system prune --volumes`
5. then get the ids for the images: `sudo docker image ls`
6. then delete the images: `sudo docker rmi -idofimage-`
  *Side Note:* This is important: You need to do this for all Dockers & Images because otherwise they will just take up space on your server.


[Back to top](#install-amcat-in-your-own-server)
