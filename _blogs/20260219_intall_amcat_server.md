---
title: "Install AmCAT in your own server"
date: 2024-11-18T12:33:46+10:00
---

Written by Jana Bernhard-Harrer and Sofia Gil-Clavel.

-   [Index:](#install-amcat-in-your-own-server)
    -   [Step 1: Connect to your server and log in](#step-1-connect-to-your-server-and-log-in)
    -   [Step 2: Open a port to/from your server](#step2-open-a-port-to-from-your-server)
    -   [Step 3: Install Docker](#step-3-install-docker)
    -   [Step 4: Install AmCAT](#step-4-install-amcat)


# Install AmCAT in your own server

## Step 1: Connect to your server and log in

`ssh <username>@<serveraddress>`

## Step 2: Open a port to/from your server (*so that it can communicate with the outside world*)

`sudo iptables -I OUTPUT -j ACCEPT`
`sudo ip6tables -I OUTPUT -j ACCEPT`

*Side Note:* sudo tells the server that you have admin rights. 

## Step 3: Install Docker (we will use this to install AmCAT)

1. check which ubunto version you have to know which Docker you need: `cat /etc/os-release` 
2. go to: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository and follow the instructions 

	1. set up Docker's apt repository 
	  *Side Note:* `apt` is like `conda install` 
	2. install the latest Docker packages
	3. check if the Docker is running with: 
	  `sudo systemctl status docker` 
	4. verify that the installation is successful 
	3. in case you need to delete a Docker you need to follow a few steps:
	1. first get the names / ids of all the dockers with `sudo docker ps`
	2. then stop the dockers: `sudo docker stop -nameofdocker-`
	3. then remove the containers: `sudo docker rm -nameofdocker-`
	4. then prune everything: `sudo docker system prune --volumes`
	5. then get the ids for the images: `sudo docker image ls`
	6. then delete the images: `sudo docker rmi -idofimage-`
	  *Side Note:* This is important: You need to do this for all Dockers & Images because otherwise they will just take up space on your server.

## Step 4: Install AmCAT

1. go to: https://github.com/ccs-amsterdam/amcat4docker 
2. decide which of the AmCAT Versions you want and download the correct docker file (Options include: the stable, the newest or one you can make available through the internet.) We go forward with one that can be made available through the internet. 
3. create a directory where AmCAT can life on your server and go there
   `mkdir amcat && cd amcat`
4. load the right dockerfile. For us:
   `wget https://raw.githubusercontent.com/ccs amsterdam/amcat4docker/main/docker-compose-https.yml`
5. install nano if you do not have a textedit program:
	1. for this you can use `sudo apt nano`
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


[Back to top](#install-amcat-in-your-own-server)
