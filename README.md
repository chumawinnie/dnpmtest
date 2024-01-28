# DNPM TEST SETUP

## Setting Up DNPM on  SIngle Nginx Server

### PREREQUISITES & SET UP NEEDED
- AWS SERVER - UBUNTU 22 WAS USED HERE
- NGINX PROXY SECURED WITH SSL
- JAVA 11
- Docker  and Docker compose

#### CONNECT TO THE NODE
  Connect to the EC2 instance - ssh -i dnpmnode.pem  ubuntu@18.236.71.114
> notes: unlink old symbolic link--   `sudo unlink /etc/nginx/sites-enabled/example`

> link new - `sudo ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/`



#### Generate cert files
  ```
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
  -nodes -keyout dnpmtest.com.key -out dnpmtest.com.crt -subj "/CN=dnpmtest.com" \
  -addext "subjectAltName=DNS:dnpmtest.com,DNS:*.dnpmtest.com,IP:18.236.71.114"
```

#### Generate ssl using Letsencrypt   

```bash
sudo apt install certbot python3-certbot-nginx
certbot --nginx -d dnpmtest.com -d www.dnpmtest.com --register-unsafely-without-email

``` 

#### Setting up java and nginx
 
 ``` bash
  sudo apt update
  sudo apt upgrade
  sudo apt install openjdk-11-jre
  sudo apt install nginx
  sudo systemctl start nginx
  sudo systemctl enable nginx
# you may need to allow some port
  sudo ufw allow 80

```
> notes: always do a - `sudo nginx -t`  to confirm  and `sudo systemctl reload nginx` especially when a change is made

####  Install docker and docker compose 

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
docker --version #confirm docker version

#install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version

```


### Login
1. login to AWS server using ` ssh -i <path to the pem file>  ubuntu@34.214.169.252 `
2. `cd  /home/dnpm`  - this directory uses 2 containers and nginx running on the box
3. confirm that nginx is active using  - `systemctl status nginx` and if not active - `systemctl start nginx`


### login into a running container

run this `docker ps` to show you the running docker container

run  `docker exec -it <containerid>  /bin/bash ` 

### Status 
 `systemctl status nginx ` <br>
 `systemctl start nginx`

 ## docker commands 
 Docker Compose works from the `/home/dnpm`<br>
 `docker compose up -d `  - to bring up the containers <br>
 `docker compose down`    - to destroy  down

### Checking connection 
`curl http://localhost:9000/bwhc` <br>
`curl http://localhost:3000` <br>

but this isnt working `curl <http://localhost:9000/bwhc/user/api/login`

