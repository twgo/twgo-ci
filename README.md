# twgo-ci
Jenkins CI setting for continuous experiment

# What?
Use Jenkins to run dockerfile in docker

# Install
install docker and docker-compose

or

run from the origin
```
docker run -d --restart=always --name myjenkins -p 80:8080 -p 50000:50000 --env JAVA_OPTS=-Dhudson.timezone=Asia/Taipei jenkins/jenkins:lts
```
https://github.com/jenkinsci/docker

## make user own docker

Try running

```
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "/home/$USER/.docker" -R
```
$USER is the username of the currently logged in user.
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
Docker (plugin https://plugins.jenkins.io/docker-plugin )

## Set docker
```
ubuntu 16.04
Inside file /lib/systemd/system/docker.service change:
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375

and run:

pkill docker
systemctl daemon-reload
systemctl start docker.service
docker-compose up
```
ref: https://github.com/portainer/portainer/issues/460

## Set docker cloud in Jenkins
管理 Jenkins > 組態設定，最下面 > 新增雲 docker
uri `tcp://YOUR_JENKINS_IP:2375`
進階
Docker Hostname or IP address	 `http://YOUR_JENKINS_IP`

## Let All Auto-build
佇組態的原始碼管理的Branches to build
configure > Branch to Build > Branch Specifier (blank for 'any')

## Sync Timezone with docker
run docker with `-v /etc/timezone:/etc/timezone -v /etc/localtime:/etc/localtime`
Check it on http://your-jenkins/systemInfo

# Self-host docker image
## launch registery server
```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```
## 'client' setting (which use docker pull)
```
# /etc/docker/daemon.json
 { "insecure-registries":["myregistry.example.com:5000"] }
```
# Warning
- use new branch name: Dokcer will use cache if you didn't edit your dockerfile/ docker-compose, be sure you didn't change your clone repo, use new branch name is better.
- If you use VM like `openstack`, open the port for your services.
