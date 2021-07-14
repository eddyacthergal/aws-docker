# aws-docker-config
AWS Docker Config

Amazon changed the install in Linux 2. One no-longer using 'yum'
See: https://aws.amazon.com/amazon-linux-2/release-notes/

## Docker Install

```sh
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```

Make docker auto-start

`sudo chkconfig docker on`

Reboot to verify it all loads fine on its own.

`sudo reboot`

## docker-compose install

Copy the appropriate `docker-compose` binary from GitHub:

`sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose`

NOTE: to get the latest version (thanks @spodnet):
`sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose`

Fix permissions after download: 

`sudo chmod +x /usr/local/bin/docker-compose`
`sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`

Verify success: 

`docker-compose version`


## Git install

`sudo yum install -y git`