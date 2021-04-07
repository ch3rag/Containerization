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
## Image Commands
---
### ```docker images```
* List images.
* -q, --quiet: Only show image IDs.
*  -a, --all: Show all images (default hides intermediate images).
```bash

chirag@ubuntu:~$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
alpine       latest    49f356fa4513   6 days ago    5.61MB
nginx        latest    7ce4f91ef623   7 days ago    133MB
chirag@ubuntu:~$ docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
alpine       latest    49f356fa4513   6 days ago    5.61MB
nginx        latest    7ce4f91ef623   7 days
chirag@ubuntu:~$ docker images -aq
49f356fa4513
7ce4f91ef623
```
---
### ```docker rmi```
* Remove one or more images.
* -f, --force: Force removal of the image
```bash
chirag@ubuntu:~$ docker rmi alpine:latest
Untagged: alpine:latest
Untagged: alpine@sha256:ec14c7992a97fc11425907e908340c6c3d6ff602f5f13d899e6b7027c9b4133a
Deleted: sha256:49f356fa4513676c5e22e3a8404aad6c7262cc7aaed15341458265320786c58c
Deleted: sha256:8ea3b23f387bedc5e3cee574742d748941443c328a75f511eb37b0d8b6164130
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
```
chirag@ubuntu:~$ docker run --name webserver --detach --publish 80:80 --network my_app_net nginx
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
chirag@ubuntu:~$ docker ps
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
* Equvalent to ```docker exec```.
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
