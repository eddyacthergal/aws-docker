# Docker Configuration for EC2 (AWS)

Amazon changed the install in Linux 2. One no-longer using 'yum'
See: https://aws.amazon.com/amazon-linux-2/release-notes/

## 1. Docker Install

```sh
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```

Make docker auto-start

```sh
sudo chkconfig docker on
```

Reboot to verify it all loads fine on its own.

```sh
sudo reboot
```

## 2. Docker-compose Install

Copy the appropriate `docker-compose` binary from GitHub:

```sh
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-`uname -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose > /dev/null
```

Fix permissions after download: 

```sh
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Verify success: 

`docker-compose version`


## 3. Git install

`sudo yum install -y git`

Clone projet on ec-user home directory (/home/ec2-user) `git clone https://github.com/eddyacthergal/aws-docker.git` and access the project (/home/ec2-user/aws-docker)

## 4. Creating / Managing Docker containers

An docker-compose configuration file (/home/ec2-user/aws-docker/docker-compose) is proposed to build build container with differents services:
 - tomcat 8.5 (with EP Config)
 - MySQL database (lightweight)
 - Nginx
 - Others services can be added (apache artemis, )

Service's Volumes are pre-configured (/home/ec2-user/aws-docker/docker-storage) to map internal containers' volumes.

In docker-compose directory:
 - Create / Update services: `sudo docker-compose up --no-start` (not starting) or `sudo docker-compose up -d` (start in background)
 - Start / Stop services: `docker-compose start`, `docker-compose stop`. Or just `docker-compose restart` to restart all services

Docker commands:
 - view running container(s): `docker ps`  or `docker container ls` 
 - view running container(s) stats: `docker stats`  

Manage individual service (knowing service name's):
 - Start / Stop:  `docker container start image-nginx` , `docker container stop image-nginx`  or `docker container restart image-nginx` 
 

## 5. Service Configuration

All services have volumes configured and accessible under `/home/ec2-user/aws-docker/docker-storage`:
 - tomcat-storage: deploy war(s), access logs, configure application(s), users, librairies ...
 - mysql-storage: if required (can be backed-up)
 - nginx-storage: configuration files, logs, ...

