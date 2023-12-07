# dnpmtest
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

