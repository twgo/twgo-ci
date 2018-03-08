# twgo-ci
CI codes

# What?
Use Jenkins to run dockerfile in docker

# Install
install docker and docker-compose

# Usage
## Run Jenkins
`docker-compose up`
Will launch Jenkins with port 80

## get YOUR_CONTAINER_ID
`docker ps`

# Setup

## admin password
`docker exec YOUR_CONTAINER_ID cat /var/jenkins_home/secrets/initialAdminPassword`


## Create a new admin user

## install Jenkins Plugin
Docker plugin

## Set docker
```
https://unix.stackexchange.com/questions/411013/connecting-to-a-docker-host/411018

# /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375

# /etc/default/docker
DOCKER_OPTS="-H tcp://0.0.0.0:2375"

# restart
sudo service docker start

source: https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-is-the-docker-daemon-running-on-this-host/8925/17
```

## Set docker cloud in Jenkins
管理 Jenkins > 組態設定，最下面 > 新增雲 docker
uri `tcp://YOUR_JENKINS_IP:2375`
進階
Docker Hostname or IP address	 `http://YOUR_JENKINS_IP`

## Let 
佇組態的原始碼管理的Branches to build
configure > Branch to Build > Branch Specifier (blank for 'any')	

# Warning
- use new branch name: Dokcer will use cache if you didn't edit your dockerfile/ docker-compose, be sure you didn't change your clone repo, use new branch name is better.
- If you use VM like `openstack`, open the port for your services.
