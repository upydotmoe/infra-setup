## Table of content

- Setup Docker
- Setup database server
- Setup supporting tools
- Troubleshoots
- Useful Links

<hr>

### # Setup Docker Engine

Install Docker CE
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

Install docker-compose
```
curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" > ./docker-compose
sudo mv ./docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

### # Setup Database Server

We will install followings:
- MariaDB

Supporting package/tools:
- NewRelic MySQL database monitoring
- Prisma Studio https://www.prisma.io/studio

#### # Installing & Starting DB Service

Make sure, Docker is already installed.

```
# git clone https://github.com/uuppyy/mysql-database-service
# cd mysql-database-service
# sudo chmod +x start.sh stop.sh restart.sh
# ./start.sh
```

* If my.cnf not working, make sure `configs/conf.d/my.cnf` has proper permission
```
chmod 0444 configs/conf.d/my.cnf
```

### # Supporting Tools

- htop
```
sudo apt install htop
```

### # Troubleshoots

- 502 Bad Gateway https://www.digitalocean.com/community/questions/nginx-gives-502-bad-gateway-when-proxying-to-nodejs-app-running-on-different-docker-container?comment=147534

### # Useful Links
- Setup SSL with Docker, NGINX and Let's Encrypt https://www.programonaut.com/setup-ssl-with-docker-nginx-and-lets-encrypt/
- Docker Compose Cheat-sheet https://devhints.io/docker-compose
- How To Secure a Containerized Node.js Application with Nginx, Let's Encrypt, and Docker Compose https://www.digitalocean.com/community/tutorials/how-to-secure-a-containerized-node-js-application-with-nginx-let-s-encrypt-and-docker-compose
