# Dockerfile Basics
**Each of the command below will result in a new layer added to the docker image.**

### ```FROM```
FROM is used to define the base image to start the build process. Every Dockerfile must start with the FROM instruction. The idea behind this is that you need a starting point to build your image.

```Dockerfile
FROM ubuntu
```
```Dockerfile
FROM debian:jessie
```
---
### ```ENV```
This command used to set the environment variables that is required to run the project.
```Dockerfile
ENV NGINX_VERSION 1.11.10-1-jessie
ENV HTTP_PORT="9000"
```
### ```WORKDIR```
The WORKDIR command is used to define the working directory of a Docker.
```Dockerfile
WORKDIR /path/to/workdir
```
---
### ```ENTRYPOINT```
* ENTRYPOINT sets the command and parameters that will be executed first when a container is run. 
* Any command-line arguments passed to ```docker run <image>``` will be appended to the ENTRYPOINT command, and will override all elements specified using CMD
* For example, ```docker run <image> bash``` will add the command argument bash to the end of the ENTRYPOINT command.
```Dockerfile
ENTRYPOINT [ "sh", "-c", "echo $HOME" ]
```
---
### ```CMD```
* Both CMD and ENTRYPOINT instructions define what command gets executed when running a container.

* The CMD instruction has three forms:
  * ```CMD ["executable","param_1","param_2"]```
  * ```CMD ["param_1","param_2"]```
  * ```CMD command param_1 param_2```
```Dockerfile
CMD ["bin/ping", "localhost"]
```
```Dockerfile
CMD ["nginx", "-g", "daemon off;"]
```
---
### ```COPY```
* The COPY command is used to copy one or many local files or folders from source and adds them to the filesystem of the containers at the destination path.

* COPY has two forms:
  * ```COPY <src>,... <dest>```
  * ```COPY ["<src>",... "<dest>"]```

```Dockerfile
COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
COPY test /absoluteDir/  # adds "test" to /absoluteDir/
```
---
### ```ADD```
* The ADD command is used to add one or many local files or folders from the source and adds them to the filesystem of the containers at the destination path.

* It is Similar to COPY command but it has some additional features:

* If the source is a local tar archive in a recognized compression format, then it is automatically unpacked as a directory into the Docker image.
* If the source is a URL, then it will download and copy the file into the destination within the Docker image. However, Docker discourages using ADD for this purpose.
* ADD has two forms:
	* ```ADD <src>... <dest>```
	* ```ADD ["<src>",... "<dest>"]```

---
### ```EXPOSE```
* The EXPOSE command informs the Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.
```Dockerfile
EXPOSE 80 443
```
---
### ```RUN```
* The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.
* Multiple shell commands are combined in single RUN instruction so that all the shell instructions fit into one single layer.
  ```Dockerfile
  RUN echo "Hello, World!" \
      && apt-get update \
      && apt-get install nginx \
                         curl \
                         openssh-server \
      && rm -rf /var/lib/apt/lists/*
  ```
* RUN has 2 forms:
  * ```RUN <command>```
  * ```RUN ["executable", "param_1", "param_2"]```
  
```Dockerfile
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
```
---
## Docker Image Logging
* Make sure to redirect the logs you want to display to stdout and stderr so that docker can manage them for you.
```Dockerfile
RUN ln -sf /dev/stdout /var/log/nginx/assess.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log
```
---