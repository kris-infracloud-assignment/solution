## List of commands run for the assignement
## The assignement was done on a AWS RHEL 8.3 VM (EC2 instance)


## Installed the below listed packages.
## Using yum - 1. docker 2. git 3. wget 
 yum install docker git wget
 
## Docker compose packages copied and configured
 curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 chmod +x /usr/local/bin/docker-compose
 ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
 docker-compose --version

## Created an empty repo on GIT and cloned to local VM
 git clone https://github.com/kris-infracloud-assignment/solution.git

## Change directory to the solutions folder
 cd solution/

## 1.1.1 Create Docker file to create the image with image from 'infracloudio/csvserver:latest'
 vi Dockerfile
 
## 1.1.2 Build image with the docker build command, named the iamge csvserver
 docker build --tag csvserver .

## 1.1.3 Check the images using the docker images command
 docker images
 
## 1.1.4 Create a container with the image "csvserver" name it "csvserver".
 docker run -d localhost/csvserver --name csvserver

## 1.2.1 Check for the running containers
 docker ps
 
## 1.2.2 No container found running, checking for stopped containers 
 docker ps -a
 
## 1.2.3 Check logs for the container
 docker logs 44c0c0f39012

## 1.2.4 Log had an error message 
## "error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory"
## Trying to provide a dummy inputdata file, updating the Dockerfile
 vi Dockerfile

## 1.2.5 Create dummy file inputdate
 touch inputdata

## 1.2.6 Build new image with the updated Dockerfile 
 docker build --tag csvserver_v1 .

## 1.2.7 Check the docker images 
 docker images

## 1.2.8 Create container with the new image with name csvserver_v1
 docker run --name csvserver_v1 -d localhost/csvserver_v1

## 1.2.9 Check the logs for the newly created container
 docker logs ed149c49dd41

## 1.3.1 Create gencsv.sh script to create inputFile as per the requirement
 vi gencsv.sh
 chmod +x gencsv.sh
 ./gencsv.sh
 cat inputFile
 vi gencsv.sh
 ./gencsv.sh
 cat inputFile
 rm inputFile

## 1.3.2 Test the script to create 1Lakh entries
  vi gencsv.sh
 ./gencsv.sh
 cat inputFile
 rm inputFile
 
## 1.3.3 Reset the script to generate 10 numbers 
 vi gencsv.sh
 

## 1.4.1 Update Dockerfile to use the inputfile, copy it to location /csvserver with the name inputdata
 vi Dockerfile
 
## 1.4.2 Build image with the updated Dockerfile with the tag csvserver_v2
 docker build --tag csvserver_v2 .
 
## 1.4.3 Run container with the new image and name it as csvserver_v3
 docker run --name csvserver_v3 -d localhost/csvserver_v2
 docker ps

## 1.5.1 To check for the port thats being used, connect to the container and run netstat -tulpn
 docker exec -it c8bb762c4f15 bash

## 1.5.2 Check for all containers and remove them
 docker ps -a
 docker rm c8bb762c4f15
 docker rm -f c8bb762c4f15
 docker rm -f ed149c49dd41
 docker rm -f c
 docker rm -f 4
 docker ps -a

## 1.6.1 Update Dockerfile with the env variable 
 vi Dockerfile

## 1.6.2 Build image with the new Dockerfile
 docker build --tag csvserver_v6 .
 docker images

## 1.6.3 Create container with the new image
 docker run --name csvserver_v6 -d -p 9393:9300 localhost/csvserver_v6

## 1.6.4 From the browser check that the application is accessible at 9393 port, and welcome note should have orange background
