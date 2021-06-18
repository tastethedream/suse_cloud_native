# Suse Cloud Native Scholarship : Lesson 3

## Container Orchestration With Kubernetes


### Virtual Machines

In essence, a VM is composed of an *operating system* with a set of pre-installed libraries and packages. During execution, an *application* utilizes an OS filesystem, resources, and packages.

A set of VMs is managed through a* hypervisor*. A hypervisor provides the virtualization of the *infrastructure* which is composed of physical servers. As a result, a hypervisor is capable of creating, configuring, and managing multiple VMs on the available servers. For example, we are able to running applications A, B, and C on 3 separate VMs.

The utilization of VMs introduced standardization in infrastructure provisioning, in association with efficient use of available infrastructure. Instead of running an application per server, a hypervisor enables multiple VMs to run at the same time to host multiple applications. However, there is one downside to this mechanism: it is not efficient enough. For example, applications A, B, and C uses the same Operating System. Replicating an OS consumes a lot of resources, and the more applications we run the more space we allocate to the replication of the operating systems alone.

### Containers


There was a clear need to optimize the usage of the available infrastructure. As a result, the virtualization of the Operating System was introduced. This prompted the appearance of containers, which represent the bare minimum an application requires for a successful execution e.g. code, config files, and dependencies. By default, there is a better usage of available infrastructure.

Multiple VMs on a hypervisor are replaced by multiple containers running on a single host operating system. The processes in the containers are completely isolated but are able to access the OS filesystem, resources, and packages. The creation and execution of containers is delegated to a container management tool, such as Docker.

The appearance of containers is unlocked by OS-level virtualization and as a result, multiple applications can run on the same OS. By nature, containers are lightweight, as these encapsulate only the application code and essential dependencies. Consequently, there is a better usage of available infrastructure and a more efficient pathway to release a product to consumers.

### Docker For Application Packaging

The appearance of containers enabled organizations to ship products using a lightweight mechanism, that would make the most of available infrastructure. There are plenty of tools used to containerize services, however, Docker has set the industry standards for many years.

To containerize an application using Docker, 3 main components are distinguished: Dockerfiles, Docker images, and Docker registries.

#### Dockerfile

A Dockerfile is a set of instructions used to create a Docker image. Each instruction is an operation used to package the application, such as installing dependencies, compile the code, or impersonate a specific user. A Docker image is composed of multiple layers, and each layer is represented by an instruction in the Dockerfile. All layers are cached and if an instruction is modified, then during the build process only the changed layer will be rebuild. As a result, building a Docker image using a Dockerfile is a lightweight and quick process.

 #####  Create the Dockerfile

 - Create the file inside the working directory(the same one as your application)

 `vi Dockerfile`

 - Inside the Dockerfile 
 ```
 FROM golang:alpine

# Set the Current Working Directory inside the container
WORKDIR /go/src/app 

# Copy everything from the current directory to the PWD(Present Working Directory) inside the container
COPY . /go/src/app  

# Download all the dependencies

# Install the package
RUN go build -o helloworld

# This container exposes port 8080 to the outside world
EXPOSE 6111

# Run the executable
CMD ["./helloworld"]

 ``` 

- Save and quit

- Create a go.mod file

`vi go.mod`

- Add the following to the file

```
module example.com/main

go 1.12
```

this file shows the location of the application code and the version of Go.

##### Docker Image

- Build the image

`docker build -t go-helloworld .`

- Run the app

`docker run -d -p 6111:6111 go-helloworld`


- To see the image you have created 

`docker images`

- Open localhost 6111 in the browser to see the application running 

##### Docker Registry

The last step in packaging an application using Docker is to store and distribute it. So far, we have built and tested an image on the local machine, which does not ensure that other engineers have access to it. As a result, the image needs to be pushed to a *public Docker image registry*, such as DockerHub, Harbor, Google Container Registry, and many more. However, there might be cases where an image should be private and only available to trusted parties. As a result, a team can host private image registries, which provides full control over who can access and execute the image.

Before pushing an image to a Docker registry, it is highly recommended to tag it first. During the build stage, if a tag is not provided (via the -t or --tag flag), then the image would be allocated an ID, which does not have a human-readable format (e.g. 0e5574283393). On the other side, a defined tag is easily scalable by the human eye, as it is composed of a registry repository, image name, and version. Also, a tag provides version control over application releases, as a new tag would indicate a new release.

To tag an existing image on the local machine, the `docker tag` command is available. Below is the syntax for this command:

`docker tag go-helloworld dockerhub_ repo_name_here/go-helloworld:v1.12`


Note: You will need to use docker login to login into Docker before pushing images to DockerHub.

#### Dockerising a Node.js web app

- Create a new directory 

`mkdir docker_node_example`

- Create a package.json

`vi package.json`

- Populate package.json

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

- run `npm install` to create your package-lock.json

- create a new file `server.js`

```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);

```

This is an example app, your application code would be here

- Create the Docker file

`vi Dockerfile`

- Define from what image we want to build

`FROM node:14`

- Create a directory to hold the application code (your working directory)
`# Create app directory
WORKDIR /usr/src/app`

- Install the app dependencies using the npm binary

```
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production
```
- Bundle your allication source code inside the Docker image

```
# Bundle app source
COPY . .
```
- Expose the port you wish to use

`EXPOSE 8080`

- Define the run command

`CMD [ "node", "server.js" ]`

- Save and quick Dockerfile

- Create `.dockerignore`

`vi .dockerignore`

- Add the following content
```
node_modules
npm-debug.log
```

This will prevent your local modules and debug logs from being copied onto your Docker image and possibly overwriting modules installed within your image.

- save and quit `.dockerignore`

- - Build your image

`docker build . -t node-web-app`

- Check the image list

`docker images`

- Run Your Image

`docker run -p 49160:8080 -d node-web-app`

- Check local host 49160 to see the app running in the container.

- To stop one or more containers

`docker stop $(docker ps -aq)`

- To remove one or more containers

`docker rm $(docker ps -aq)`

- To check this is complete or to check what is running at anytime

`docker ps -a`



