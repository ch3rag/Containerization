# Docker Commands
## Basic Commands
---
### ```docker version```
* Print version information.
```bash
chirag@ubuntu:~$ clear
chirag@ubuntu:~$ docker version
Client: Docker Engine - Community
 Version:           20.10.5
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        55c4c88
...
```
---
### ```docker info```
* This command displays system wide information regarding the Docker installation. 
* Information displayed includes the kernel version, number of containers and images. 
* The number of images shown is the number of unique images. The same image tagged under different names is counted only once.

```bash
chirag@ubuntu:~$ docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)

Server:
 Containers: 2
  Running: 2
  Paused: 0
  Stopped: 0
 Images: 5
 Server Version: 20.10.5
...
```
---
### ```docker login```
* Log in to a Docker registry.
* -p, --password string: Password.
* -u, --username string: Username.
```bash
chirag@ubuntu:~$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: your_username
Password:
WARNING! Your password will be stored unencrypted in /home/chirag/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```
---
### ```docker logout```
* Log out from a Docker registry.
```bash
chirag@ubuntu:~$ docker logout
Removing login credentials for https://index.docker.io/v1/
```
---
### ```docker search```
* Search the Docker Hub for images.
```bash
chirag@ubuntu:~$ docker search jekyll-serve
NAME                                    DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
bretfisher/jekyll-serve                 The Latest Jekyll in a Docker Container For …   88     
...
````
---
## Image Commands
---
### ```docker image ls```
* List images.
* Equivalent to ```docker images```.
* -q, --quiet: Only show image IDs.
*  -a, --all: Show all images (default hides intermediate images).
```bash
chirag@ubuntu:~$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    26b77e58432b   5 days ago    72.9MB
alpine       latest    49f356fa4513   7 days ago    5.61MB
...
```
---
### ```docker image pull```
* Pull an image or a repository from a registry.
* Equivalent to ```docker pull```.
```bash
chirag@ubuntu:~$ docker image pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete
Digest: sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```
---
### ```docker image push```
* Push an image or a repository to a registry
* Equivalent to ```docker push```.
```bash
chirag@ubuntu:~$ docker image push ch3rag/nginx
Using default tag: latest
The push refers to repository [docker.io/ch3rag/nginx]
1914a564711c: Mounted from library/nginx
db765d5bf9f8: Mounted from library/nginx
903ae422d007: Mounted from library/nginx
66f88fdd699b: Mounted from library/nginx
2ba086d0a00c: Mounted from library/nginx
346fddbbb0ff: Mounted from library/mysql
latest: digest: sha256:c137f6c852bfdf74694fe20693bb11e61b51e0b8c50d17dff881f2db05e65de9 size: 1570
```
---
### ```docker image rm```
* Remove one or more images.
* Equivalent to ```docker rmi```.
* -f, --force: Force removal of the image
```bash
chirag@ubuntu:~$ docker image rm hello-world:latest
Untagged: hello-world:latest
Untagged: hello-world@sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726
Deleted: sha256:f22b99068db93900abe17f7f5e09ec775c2826ecfe9db961fea68293744144bd
```
---
### ```docker image history```
* Show the history of an image.
* Equivalent to ```docker history```.
```bash
chirag@ubuntu:~$ docker image history ubuntu:latest
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
26b77e58432b   5 days ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      5 days ago   /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B
<missing>      5 days ago   /bin/sh -c [ -z "$(apt-get indextargets)" ]     0B
<missing>      5 days ago   /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   811B
<missing>      5 days ago   /bin/sh -c #(nop) ADD file:27277aee655dd263e…   72.9MB
```
---
### ```docker image inspect```
* Display detailed information on one or more images.
```bash
chirag@ubuntu:~$ docker image inspect nginx:latest
[
    {
        "Id": "sha256:7ce4f91ef623b9672ec12302c4a710629cd542617c1ebc616a48d06e2a84656a",
        "RepoTags": [
            "nginx:latest"
        ],
        ...
        "ContainerConfig": {
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.19.9",
                "NJS_VERSION=0.5.3",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            ...
```
---
### ```docker image tag```
* Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE.
* Equivalent to ```docker tag```.
```bash
chirag@ubuntu:~$ docker image ls | grep nginx
ch3rag/mynginx   latest    7ce4f91ef623   9 days ago     133MB
nginx            latest    7ce4f91ef623   9 days ago     133MB
```
---
### ```docker image build```
* Build an image from a Dockerfile.
* Equivalent to ```docker build```.
* -t, --tag list: Name and optionally a tag in the 'name:tag' format.
* -f, --file string: Name of the Dockerfile (Default is 'PATH/Dockerfile').
```bash
chirag@ubuntu:~/nodeapp$ ls
Dockerfile  index.js  package.json
chirag@ubuntu:~/nodeapp$ nano Dockerfile
chirag@ubuntu:~/nodeapp$ clear
chirag@ubuntu:~/nodeapp$ cat index.js
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello, World!'));

app.listen(3000, () => {
        console.log('App listening on port 3000');
});
chirag@ubuntu:~/nodeapp$ cat package.json
{
        "name": "hello-world",
        "version": "1.0.0",
        "main": "index.js",
        "dependencies": {
                "express": "^4.16.2"
        },
        "scripts": {
                "start": "node index.js"
        }
}
chirag@ubuntu:~/nodeapp$ cat Dockerfile
FROM node

EXPOSE 3000

WORKDIR /app

COPY package.json index.js ./

RUN npm install

CMD ["npm", "start"]
chirag@ubuntu:~/nodeapp$ docker image build -t hello-nodejs .
Sending build context to Docker daemon  4.096kB
Step 1/6 : FROM node
latest: Pulling from library/node
00168f89dbe8: Pull complete
da61ad49fa99: Pull complete
af62e04c0f85: Pull complete
aae835e2c68e: Pull complete
5254dc317968: Pull complete
b3f76fdfa8e8: Pull complete
af702d43bbb8: Pull complete
6046c161e0b2: Pull complete
314cc5119377: Pull complete
Digest: sha256:bb63820d474ced424af2b038edd629056cb7b27bffef74566f872c44d588511c
Status: Downloaded newer image for node:latest
 ---> 6e72986b1b6e
Step 2/6 : EXPOSE 3000
 ---> Running in 645b30700658
Removing intermediate container 645b30700658
 ---> 84f2eee8a7c7
Step 3/6 : WORKDIR /app
 ---> Running in 9d18ec4dc71c
Removing intermediate container 9d18ec4dc71c
 ---> 5772e9091e4d
Step 4/6 : COPY package.json index.js ./
 ---> c3995860f6bd
Step 5/6 : RUN npm install
 ---> Running in 67d01d9116b7

added 50 packages, and audited 51 packages in 3s

found 0 vulnerabilities
npm notice
npm notice New minor version of npm available! 7.7.6 -> 7.9.0
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v7.9.0>
npm notice Run `npm install -g npm@7.9.0` to update!
npm notice
Removing intermediate container 67d01d9116b7
 ---> d6f909c5fc64
Step 6/6 : CMD ["npm", "start"]
 ---> Running in 5fac04004eef
Removing intermediate container 5fac04004eef
 ---> 38019b10e56a
Successfully built 38019b10e56a
Successfully tagged hello-nodejs:latest
chirag@ubuntu:~/nodeapp$ docker container logs nodeapp

> hello-world@1.0.0 start
> node index.js

App listening on port 3000
chirag@ubuntu:~/nodeapp$ curl localhost:3000
Hello, World!
```
---
### ```docker image prune```
* Remove unused images.
* -a, --all: Remove all unused images, not just dangling ones.
* -f, --force: Do not prompt for confirmation.
```bash
chirag@ubuntu:~$ docker image prune
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Total reclaimed space: 0B
```

---
## Container Commands
---
### ```docker container run```
* Run a command in a new container.
* Equivalent to ```docker run```
* -d, --detach: Run container in background and print container ID.
* -e, --env list: Set environment variables.
* --name string: Assign a name to the container.
* -p, --publish list: Publish a container's port(s) to the host.
* -t, --tty: Allocate a pseudo-TTY.
* -i, --interactive: Keep STDIN open even if not attached.
* -v, --volume list: Bind mount a volume.
* -w, --workdir string: Working directory inside the container.
* --network network: Connect a container to a network.
* --network-alias list: Add network-scoped alias for the container.
```
chirag@ubuntu:~$ docker container run --name webserver --detach --publish 80:80 --network my_app_net nginx
f77c10bbb11c6fb38e215983e6aa14dd9b8791420ee31ccc7e33aa3b30abe154
```
---
### ```docker container ls```
* List containers.
* Equivalent to ```docker ps```
* -a, --all: Show all containers (default shows just running).
* -q, --quiet: Only display container IDs
```bash
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                NAMES
f77c10bbb11c   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:80->80/tcp   webserver
chirag@ubuntu:~$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                NAMES
f77c10bbb11c   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:80->80/tcp   webserver
chirag@ubuntu:~$ docker container ls -aq
f77c10bbb11c
```
---
### ```docker container rm```
* Remove one or more containers.
* Equivalent to ```docker rm```
* -f, --force: Force the removal of a running container (uses SIGKILL).
```bash
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
099400ce91fc   httpd     "httpd-foreground"       17 seconds ago   Up 16 seconds   0.0.0.0:8080->80/tcp   proxy
15ad330bf182   nginx     "/docker-entrypoint.…"   43 seconds ago   Up 42 seconds   0.0.0.0:80->80/tcp     webserver
chirag@ubuntu:~$ docker container rm -f proxy
proxy
chirag@ubuntu:~$ docker container rm -f $(docker container ls -aq)
15ad330bf182
chirag@ubuntu:~$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
---
### ```docker container stop```
* Stop one or more running containers.
* Equivalent to ```docker stop```.
* -t, --time int: Seconds to wait for stop before killing it (default 10).
```bash
chirag@ubuntu:~$ docker container run --detach --name webserver --publish 80:80 nginx
1b0d4c6252feceb42964c7d3496f3eef68262395474a7c3ac54c54588e7281af
chirag@ubuntu:~$ docker stop webserver
webserver
chirag@ubuntu:~$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS     NAMES
1b0d4c6252fe   nginx     "/docker-entrypoint.…"   15 seconds ago   Exited (0) 9 seconds ago             webserver
```
---
### ```docker container start```
* Start one or more stopped containers.
* -a, --attach: Attach STDOUT/STDERR and forward signals.
* -i, --interactive: Attach container's STDIN
```bash
chirag@ubuntu:~$ docker container start -ai webserver
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: IPv6 listen already enabled
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
```
### ```docker container inspect```
* Display detailed information on one or more containers.
* -f, --format string: Format the output using the given Go template.
```bash
chirag@ubuntu:~$ docker inspect webserver
[
    {
        "Id": "1b0d4c6252feceb42964c7d3496f3eef68262395474a7c3ac54c54588e7281af",
        "Created": "2021-04-07T08:48:42.951220289Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
        ...
		"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "a67e663ec1fdd19274b0057d492816acffc75197fc50daa7966013fcbba93c9f",
                    "EndpointID": "36cafd2810929f21cae2ba384ce3e4452917e396f5b725042f58f03a557066a2",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    ...
chirag@ubuntu:~$ docker inspect webserver -f "{{.NetworkSettings.IPAddress}}"
172.17.0.2
```
---
### ```docker container exec```
* Run a command in a running container.
* Equivalent to ```docker exec```.
* -d, --detach: Detached mode: run command in the background.
* -i, --interactive: Keep STDIN open even if not attached.
* -t, --tty: Allocate a pseudo-TTY.
* -w, --workdir string: Working directory inside the container.
* -e, --env list: Set environment variables.
```
chirag@ubuntu:~$ docker container run --name webserver --detach nginx
40797c879e3951af276737132eacd655cd95f5b17c8e2e483b8f0b7bb59f2c8b
chirag@ubuntu:~$ docker container exec -it -w /home -e MY_NAME=CHIRAG webserver bash
root@40797c879e39:/home# echo "$MY_NAME"
CHIRAG
```
---
### ```docker container logs```
* Fetch the logs of a container.
* Equivalent to ```docker logs```.
* -f, --follow: Follow log output.
* -n, --tail string: Number of lines to show from the end of the logs (default "all").
* -t, --timestamps: Show timestamps.
* --until string: Show logs before a timestamp.
```bash
chirag@ubuntu:~$ docker container logs webserver
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
chirag@ubuntu:~$ docker container logs webserver --tail 2
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
```
---
### ```docker container top```
* Display the running processes of a container.
* Equivalent to ```docker top```.
```bash
chirag@ubuntu:~$ docker container top webserver
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                20692               20660               0                   14:33               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            20763               20692               0                   14:33               ?                   00:00:00            nginx: worker process
```
---
### ```docker container stats```
* Display a live stream of container(s) resource usage statistics.
* Equivalent top ```docker stats```.
* -a, --all: Show all containers (default shows just running).
```bash
chirag@ubuntu:~$ docker container stats
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
9c82b9649a9b   apache      0.00%     5.848MiB / 1.946GiB   0.29%     586B / 0B     0B / 0B       82
40797c879e39   webserver   0.00%     2.273MiB / 1.946GiB   0.11%     1.19kB / 0B   0B / 8.19kB   2
...
chirag@ubuntu:~$ docker container stats webserver
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
40797c879e39   webserver   0.00%     2.273MiB / 1.946GiB   0.11%     1.19kB / 0B   0B / 8.19kB   2
```
---
### ```docker container pause```
* Pause all processes within one or more containers.
* Equivalent to ```docker pause```.
```bash
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
40797c879e39   nginx     "/docker-entrypoint.…"   38 minutes ago   Up 38 minutes   80/tcp    webhost
chirag@ubuntu:~$ docker container pause webhost
webhost
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                   PORTS     NAMES
40797c879e39   nginx     "/docker-entrypoint.…"   38 minutes ago   Up 38 minutes (Paused)   80/tcp    webhost
```
---
### ```docker container unpause```
* Unpause all processes within one or more containers.
* Equivalent to ```docker unpause```.
```bash
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                   PORTS     NAMES
40797c879e39   nginx     "/docker-entrypoint.…"   40 minutes ago   Up 40 minutes (Paused)   80/tcp    webhost
chirag@ubuntu:~$ docker container unpause webhost
webhost
chirag@ubuntu:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
40797c879e39   nginx     "/docker-entrypoint.…"   40 minutes ago   Up 40 minutes   80/tcp    webhost
```
---
### ```docker container rename```
* Rename a container.
```bash
chirag@ubuntu:~$ docker container rename webserver webhost
chirag@ubuntu:~$ docker container ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
9c82b9649a9b   httpd     "httpd-foreground"       20 minutes ago   Up 20 minutes   80/tcp    apache
40797c879e39   nginx     "/docker-entrypoint.…"   30 minutes ago   Up 30 minutes   80/tcp    webhost
```
---
### ```docker container kill```
* Kill one or more running containers.
#### ```docker stop``` v/s ```docker kill```
* docker stop: Stop a running container (send SIGTERM, and then SIGKILL after grace period).
* docker kill: Kill a running container (send SIGKILL, or specified signal).
```bash
chirag@ubuntu:~$ docker kill webhost
webhost
```
---
## Network Commands
---
### ```docker network ls```
* List networks.
* -q, --quiet: Only display network IDs.
```bash
chirag@ubuntu:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a67e663ec1fd   bridge    bridge    local
0105f89b0e71   host      host      local
3289b6688719   none      null      local
```
---
### ```docker network create```
* Create a network.
```bash
chirag@ubuntu:~$ docker network create my_app_net
e7a21caf02fc70071877c130324ec4fa4b5a561b211a1555d0471aa2603e00f1

```
---
### ```docker network rm```
* Remove one or more networks.
```bash
chirag@ubuntu:~$ docker network rm my_app_net
my_app_net
chirag@ubuntu:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a67e663ec1fd   bridge    bridge    local
0105f89b0e71   host      host      local
3289b6688719   none      null      local
```
---
### ```docker network inspect```
* Display detailed information on one or more networks.
```bash
chirag@ubuntu:~$ docker network inspect my_app_net
[
    {
        "Name": "my_app_net",
        "Id": "fa3f2823d5a2566abd768acc0c880c1282487f05f65a90ce3bdaeaec9a17d70f",
        "Created": "2021-04-07T14:58:47.738584312+05:30",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                    ...
```
---
### ```docker network connect```
* Connect a container to a network.
```bash
chirag@ubuntu:~$ docker container inspect webserver -f {{.NetworkSettings.Networks}}
map[bridge:0xc000503b00]
chirag@ubuntu:~$ docker network connect my_app_net webserver
chirag@ubuntu:~$ docker container inspect webserver -f {{.NetworkSettings.Networks}}
map[bridge:0xc000501b00 my_app_net:0xc000501bc0]
```
---
### ```docker network disconnect```
* Disconnect a container from a network.
* -f, --force: Force the container to disconnect from a network.
```bash
chirag@ubuntu:~$ docker container inspect webserver -f {{.NetworkSettings.Networks}}
map[bridge:0xc000501b00 my_app_net:0xc000501bc0]
chirag@ubuntu:~$ docker network disconnect my_app_net webserver
chirag@ubuntu:~$ docker container inspect webserver -f {{.NetworkSettings.Networks}}
map[bridge:0xc000505b00]
```
---
## Volume Commands
---
### ```docker volume ls```
```bash
chirag@ubuntu:~$ docker container run -d -e MYSQL_RANDOM_ROOT_PASSWORD=True --name db mysql
ceb23b63438555dad42fbe583416015ca163b612e42296aacc5827d5026cd743
chirag@ubuntu:~$ docker image inspect mysql:latest
[
    {
        ...
        "Config": {
            "Volumes": {
                "/var/lib/mysql": {}
            },
            ...
chirag@ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675
chirag@ubuntu:~$ docker container inspect db
[
    {
        ...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675",
                "Source": "/var/lib/docker/volumes/8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        ....
chirag@ubuntu:~$ docker volume inspect 8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675
[
    {
        "CreatedAt": "2021-04-09T13:37:26+05:30",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675/_data",
        "Name": "8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675",
        "Options": null,
        "Scope": "local"
    }
]
chirag@ubuntu:~$ sudo ls /var/lib/docker/volumes/8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675/_data
 auto.cnf        ca.pem               ib_buffer_pool  '#innodb_temp'        public_key.pem    undo_002
 binlog.000001   client-cert.pem      ibdata1          mysql                server-cert.pem
 binlog.000002   client-key.pem       ib_logfile0      mysql.ibd            server-key.pem
 binlog.index   '#ib_16384_0.dblwr'   ib_logfile1      performance_schema   sys
 ca-key.pem     '#ib_16384_1.dblwr'   ibtmp1           private_key.pem      undo_001
chirag@ubuntu:~$ # Clean Up
chirag@ubuntu:~$ docker container rm -f $(docker container ls -aq)
d626c1a822a1
chirag@ubuntu:~$ docker system prune -f --volumes
Deleted Volumes:
8bc64e487780ba69676929e005f261fe1809c56ec2c80724f74ef51a1b2e1675

Total reclaimed space: 204.1MB
chirag@ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
chirag@ubuntu:~$ # Named Volumes
chirag@ubuntu:~$ docker container  run -d --name db -v mysql-db:/var/lib/mysql mysql
b09a6805a82fdc342abe510b4a7b79122d95086194a009969b072ea3423ee35c
chirag@ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     mysql-db
```
---
### ```docker volume create```
* Create a volume.
-d, --driver string: Specify volume driver name (default "local")
```bash
chirag@ubuntu:~$ docker volume create myappdb
myappdb
chirag@ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     myappdb
local     mysql-db
chirag@ubuntu:~$ docker container run -d -v myappdb:/var/lib/mysql -e MYSQL_RANDOM_ROOT_PASSWORD=True --name db2  mysql
chirag@ubuntu:~$ docker container inspect db2
[
    {
        ...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "myappdb",
                "Source": "/var/lib/docker/volumes/myappdb/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        ...
```
---
### ```docker volume inspect```
* Display detailed information on one or more volumes.
```bash
chirag@ubuntu:~$ docker volume inspect myappdb
[
    {
        "CreatedAt": "2021-04-09T13:51:22+05:30",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/myappdb/_data",
        "Name": "myappdb",
        "Options": {},
        "Scope": "local"
    }
]
```
---
### ```docker volume rm```
* Remove one or more volumes.
* -f, --force: Force the removal of one or more volumes.
```bash
chirag@ubuntu:~$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
chirag@ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     myappdb
local     mysql-db
chirag@ubuntu:~$ docker volume rm myappdb mysql-db
myappdb
mysql-db
```
---
### ```docker volume prune```
* Remove all unused local volumes.
* -f, --force: Do not prompt for confirmation.
```bash
chirag@ubuntu:~$ docker volume create test-vol
test-vol
chirag@ubuntu:~$ docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
test-vol

Total reclaimed space: 0B
```
---
## System Commands
---
### ```docker system prune```
* Remove unused data.
* -a, --all: Remove all unused images not just dangling ones.
* f, --force: Do not prompt for confirmation.
* --volumes: Prune volumes.
```bash
chirag@ubuntu:~$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Networks:
search_net
my_app_net

Total reclaimed space: 0B
```
---
### ```docker system df```
* Show docker disk usage.
```bash
chirag@ubuntu:~$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          11        0         2.012GB   2.012GB (100%)
Containers      0         0         0B        0B
Local Volumes   6         0         505.6MB   505.6MB (100%)
Build Cache     0         0         0B        0B
```
---