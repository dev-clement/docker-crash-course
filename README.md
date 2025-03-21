# Docker crash-courses

This repository is done to making use of Docker and the followings information
about it:
* .dockerignore for not contenerized all of the files from a specific folder
* Dockerfile file that will create docker images
* docker-compose a configuration file that lets you running several services and configure them into one configuration file.

## Introduction

Docker is an application that lets the containerized application for development or for release mode easier to manage. How does it do that ? By just isolating each part of the application in a container.

Example of use: You are in a team of 7 or more people, and you are working on a node Application with node version 14, however the current version on your computer is the LTS which is the version 20, imagine that this versioning issue happens to some other peoples (which is often the case depending on your machine), so everyone has to downgrade their Node version to work on your application.
Although if you are working with docker instead of your own environment, you won’t need anything more than the application that lets you run docker and manage the container. However, that’s not the only thing to do, you’ll also have to download all the “packages” that the application needs to work on the correct version if you are not using Docker. In case of you are using docker for your application, a simple command will download and install all the prerequisites the container will use for your application

There is also the case where you have an application, you need to add it to some servers, so you’ll have to configure each environment to make your application working correctly with the machine’s environment, kind of a lot consuming, if you use one docker instead, you won’t have to do that.

A container is like a box, a box that contains all what is needed for your application to be running on a machine that contains nothing more than a way to run docker. In the image, you’ll have all the code, dependencies, binaries, that are used to make the application working. This way, the container ran the application in isolation of all your machine and configuration of it.

Difference between virtual machines and docker:
Virtual machine is isolated from the host machine, but even though it fix the same problem fixed by the Docker application, it is a way heavier, as it the Virtual machine has its own kernel and init.d
Docker is sharing the host operating system, doesn’t have neither a kernel nor an init.d on its own, is it a way more lightweight than a Virtual Machine

## Images

Images are a kind of a blueprint of a container, a docker image is a standardized package that includes all of the files, binaries, libraries and all the configuration that is needed for your container to run. Things to note about an image though:

An image is immutable. Once you create the image, you cannot modify it, what you can do instead is making a new image and add changes on top of that new image.
Images are composed of layers. Each layer represents a set of file system changes that add, remove or modify files.

These two principles let you extend or add to existing images. For instance, if you are building a python application, you can start from a python image, add additional layers to install your application’s dependencies and add your code. This lets you focus on your application, rather than python itself.

An image contains the followings:
1. Runtime environment (specific node version)
2. Application code
3. Needed dependencies (libraries, packages and so on)
4. Extra configuration (e.g. environment variable)
5. Entry point
6. Command
Each part that are listed above are layers. Images are made up of layers, and each layer is adding information to this image incrementally. Typically each layer is adding information to the image, and the order of those layers are important.

Image has its own file system that is independent from your computer.

## Volumes

A volume is a feature of docker that can allow the host to share a place to the container, it will map a specific folder of your host to your container. If something changes on this specific container into your host, that changes will be completely reflected to the container without rebuilding it.

### Use case

For instance, you create an image / container depending on an application you are developing, this way you made some modifications (minors or majors) onto the code presents in the container, instead of rebuilding the image to accept those changes, you can using volume to map the container to the folder where the changes happens and it will be looking for changes onto your folders file instead of rebuilding the image each time something change.

### Mapping a volume

You can do this mapping using the Dockerfile command, by adding an option to the docker run command, passing a “-v” option and adding host_path:container_path where host_path is the path on the host machine (for example: C:\users\...) and container_path is the path where the working directory is in your container (for example: /app), but you have to be sure that the content of the mapped directory on the host is correct, otherwise the content of the container won’t be correct neither. Because container and host (when using volumes for mapping) share the same content as once is the same as the other

### Volume through CLI

In order to map a path from the host to the container, we are doing this command while running the container:

| Command                                                           | Explanation                                                                                  |
|:---                                                               |:---                                                                                          |
| $> docker run -v <host_path>:<container_path>                     | Maps the host_path to the container_path. That means every changes that happens from the     |
|                                                                   | host_path will be directly mapped to the container_path that is in the container.            |
|:---                                                               |:----                                                                                         |
| $> docker run -v <host_path>:<container_path> -v <anonymous_path> | Doesn't map the path to a specific folder of the host computer, instead it maps the path to  |
|                                                                   | another folder that docker is managing, somewhere on your host making the content persistent |

However, this command might be a big fat for the simple changes for a simple docker mounting stuff, that’s where docker-compose will come into play.

## Docker compose

While using docker, the command to run a simple container can be extremely huge, specifying the port mapping, specifying volumes, specifying image and everything is made without any persistence. And what will happen if we have several docker containers for each part of a bigger project, we might need to run several docker containers, and adding all that configuration to each, that’s kind of cumbersome… What if we have a project with several containers such as:
MongoDB
React
Django
That would mean creating a container for each, and also means that we have to remember the configuration of each container… This is again extremely cumbersome. And if we want to run all the containers at once, that’s gonna be complicated and that’s why docker-compose is used for.

If we are using docker-compose, we will need some extra configuration to run each container correctly by adding configuration to it (port mapping, volumes, and so on….) but the command will be small and it will run all containers at once.

### Docker compose content

file docker-compose.yml is a yaml file that contains all the informations and configuration for each image,

