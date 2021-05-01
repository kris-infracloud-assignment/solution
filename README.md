# Prereqs
# install packages using yum "yum-utils" "git" "wget"
 yum install -y yum-utils git wget

# Add repos for docker, install docker engine using yum and start docker engine
	 yum-config-manager \
	>     --add-repo \
	>     https://download.docker.com/linux/centos/docker-ce.repo

	 yum install docker-ce docker-ce-cli containerd.io
	 systemctl start docker
 
# Copy docker compose binaries from the compose release repo from git, provide execute permissions and check that docker-compose command is working 
	 curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	 chmod +x /usr/local/bin/docker-compose
	 ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
	 docker-compose --version

#PART 1

# 1.1 run the container in the background, check if its running, if not check why.
	 docker run -d infracloudio/csvserver:latest
	 docker ps
	 docker ps -a

# 1.2 Check for logs to find why the container failed to run 
	docker logs 2c4cbe2c6951

# 1.3 create script "gencsv.sh"

	 vi gencsv.sh
	 chmod 700 gencsv.sh
	 ./gencsv.sh
	 cat inputFile
	 vi gencsv.sh
	 ./gencsv.sh
	 cat inputFile
	 vi gencsv.sh
	 cat inputFile

# 1.4 Run the container in the background with file generated in 1.3 available inside the container
	docker run -d -v /root/solution/inputFile:/csvserver/inputdata --name v1 infracloudio/csvserver
 
# 1.5 login into the container and find the listning port 
	 docker ps
	 docker exec -it 0b4cf6eade39 bash
			[root@0b4cf6eade39 csvserver]# netstat -tulpn
			[root@0b4cf6eade39 csvserver]# exit

	 docker ps -a
	 docker stop 0b4cf6eade39
	 docker stop 2c4cbe2c6951
	 docker rm 0b4cf6eade39
	 docker rm 2c4cbe2c6951

# 1.6 Run container with the port details, and confirm its accessible at port "9393", set env variable CSVSERVER_BORDER as Orange
	docker run -d -p 9393:9300 -e CSVSERVER_BORDER:Orange -v /root/solution/inputFile:/csvserver/inputdata --name v3 infracloudio/csvserver
