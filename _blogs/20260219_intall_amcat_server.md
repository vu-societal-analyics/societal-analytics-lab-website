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

1. Check which Ubuntu version you have to know which Docker you need: `cat /etc/os-release` .
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

1. Go to: https://github.com/ccs-amsterdam/amcat4docker.
2. Decide which of the AmCAT Versions you want and download the correct docker file (Options include: the stable, the newest, or one you can make available through the internet.) We go forward with one that can be made available through the internet. 
3. Create a directory where AmCAT can live on your server and go there. You can choose one of the following ways:
   * `mkdir amcat && cd amcat`.
   * `git clone https://github.com/ccs-amsterdam/amcat4docker`. This one should create this folder and download all the yml files there in one step, with the added bonus of being able to update later if the dockerfile changes
4. load the right dockerfile. For us:
   `wget https://raw.githubusercontent.com/ccs amsterdam/amcat4docker/main/docker-compose-https.yml`
5. install nano if you do not have a textedit program. For this you can use `sudo apt nano`.
6. run `nano docker-compose-https.yml` and add a name and the website where you want to host amcat. 
7. then spin up the docker containers with:
   `docker-compose -f docker-compose-https.yml up --pull="missing" -d` 
8. to check if it is running you can run:
	1. `sudo docker ps` -> see if it is there
	2. `curl localhost` -> check amcat frontend 
	3. `curl localhost/amcat/` -> check amcat backend
9. we also want it to be secure (https) so we also run the following:
   `docker exec -it ngincat certbot --nginx -d medem.amcat.eu -d amcat.medem.eu` This will open an editable file where you: 
	1. enter your email
	2. give it permission 
	3. decide whether you want to add your email or not
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
