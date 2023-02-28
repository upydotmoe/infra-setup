## Table of content

- [First Steps](https://github.com/uuppyy/server-setup#-first-steps)
- [Setup Docker](https://github.com/uuppyy/server-setup#-setup-docker-engine)
- [Setup database server](https://github.com/uuppyy/server-setup#-setup-database-server)
- [Setup nginx & API server](https://github.com/uuppyy/server-setup#-installing-and-serving-dockerized-api-service-bundled-with-nginx)
- [Setup web app](https://github.com/uuppyy/server-setup#-installing-and-serving-web)
- [Supporting tools](https://github.com/uuppyy/server-setup#-supporting-tools)
- [Troubleshoots](https://github.com/uuppyy/server-setup#-troubleshoots)
- [Useful Links](https://github.com/uuppyy/server-setup#-useful-links)

<hr>

### # First Steps

- Make sure ufw is enabled
```
sudo ufw status
```

if it's not enabled yet, run:
```
sudo ufw enable
```

### # Allow SSH connection port to ufw firewall
```
sudo ufw allow 22
sudo ufw allow 22/tcp
```

Make sure ufw port 80, 80/tcp, 443, 443/tcp is enabled
```
sudo ufw status
```

- Make ssh connectivity longer https://www.tecmint.com/increase-ssh-connection-timeout/

<hr>

### # Setup vsftpd

```
sudo apt update
sudo apt install vsftpd
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
sudo ufw allow 20,21,990/tcp
sudo ufw allow 40000:50000/tcp

sudo nano /etc/vsftpd.conf
echo "root" | sudo tee -a /etc/vsftpd.userlist
```

```conf
// /etc/vsftpd.conf
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES

# additional config
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO

require_ssl_reuse=NO
ssl_ciphers=HIGH

pasv_min_port=35000
pasv_max_port=40000

user_sub_token=$USER
local_root=/home/$USER

userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
```

<hr>

### # Setup Docker Engine

Install Docker CE
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

Install docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-$(uname -s)-$(uname -m)"  -o /usr/local/bin/docker-compose
sudo mv /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

<hr>

#### # Installing & Starting DB Service

Make sure, Docker is already installed.

```
# git clone https://github.com/uuppyy/db-services db
# cd db
# sudo chmod +x run.sh
# ./run.sh
```

* If my.cnf not working, make sure `configs/conf.d/my.cnf` has proper permission
```
chmod 0444 configs/conf.d/my.cnf
```

<hr>

### # Installing and Serving Dockerized API Service bundled with Nginx

- Install

**!! Note:** In case certbot failed to verify with non-ssl url `http://domain.com`, you can disable SSL/TLS mode in Cloudflare by set it to `Off (not secure)`
![image](https://user-images.githubusercontent.com/7555972/202084572-5245cde5-b290-43fc-a880-dac351e198f1.png)

Then we can clone the repo and serve it
```
git clone https://github.com/uuppyy/api
cd api
chmod +x ./deploy.sh
./deploy.sh
```

- Allow API port to ufw list
```
sudo ufw allow 2021 2021/tcp
```

Then turn the SSL/TLS mode back to `Full` or `Full (strict)`<br>
Try to access with `https://`, open browser and go to https://api.upy.moe

**!! IMPORTANT NOTE**
For now we don't use any custom certificate, we use certificate from Cloudflare, which require to enable force HTTPS and set SSL/TLS mode to Flexible.<br>

Any other Cloudflare settings:
- SSL/TLS Recommender on
- Always use HTTPS on
- HTTP Strict Transport Security (HSTS) on (enable HSTS, Max age 6 months, Apply HSTS policy to subdomains, Preload, No-Sniff Header)
- Automatic HTTPS Rewrites on

<hr>

### # Installing and Serving Web App

- Allow web port to ufw list
```
sudo ufw allow 3000 3000/tcp
```

- Deploy
```
git clone https://github.com/uuppyy/web-next web
cd web
git switch main
sudo chmod +x ./deploy.sh
./deploy.sh
```

**!! NOTES**
- If pm2 not working, try to remove -i flag from pm2 commands, this is because cluster mode causing the error

<hr>

### # Change Timezone to Asia/Jakarta

```
sudo timedatectl set-timezone Asia/Jakarta
```

### # Daily DB Backup

https://github.com/upyapp/infra-setup/blob/main/db-backups.md

- Immediate backup
```
docker run -d --restart=always --network=upy-network --name auto_bak -e DB_DUMP_BEGIN=+0 -e DB_DUMP_TARGET=/db -e DB_SERVER=dbs_maria -e DB_USER=youpie -e DB_PASS=tf4CcEuN3dENXLgs -e DB_NAMES=youpie  -v /db:/db databack/mysql-backup
```

- Scheduled backup (everyday at 9AM Jakarta time)
```
// 
```

<hr>

### # Tune DB

- Decrease the `wait_timeout`
```
SET session wait_timeout=60;
SHOW SESSION VARIABLES LIKE 'wait_timeout';
```

<hr>

### # Supporting Tools

- htop
```
sudo apt install htop
```

<hr>

### # Troubleshoots

- 502 Bad Gateway https://www.digitalocean.com/community/questions/nginx-gives-502-bad-gateway-when-proxying-to-nodejs-app-running-on-different-docker-container?comment=147534

<hr>

### # Useful Links
- Setup SSL with Docker, NGINX and Let's Encrypt https://www.programonaut.com/setup-ssl-with-docker-nginx-and-lets-encrypt/
- Docker Compose Cheat-sheet https://devhints.io/docker-compose
- How To Secure a Containerized Node.js Application with Nginx, Let's Encrypt, and Docker Compose https://www.digitalocean.com/community/tutorials/how-to-secure-a-containerized-node-js-application-with-nginx-let-s-encrypt-and-docker-compose
