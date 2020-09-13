---
title: "Docker, Docker-Compose and DDEV - Cheatsheet"
date: 2019-06-09
draft: false

# post thumb
image: "images/post/davidjguru_docker_docker_compose_ddev_tools_1.jpg"

# meta description
description: "Just a cheatsheet for working with Docker, Docker Compose and DDEV"

# taxonomies
categories: 
  - "Tooling"
tags:
  - "DDEV"
  - "Containers"
  - "Docker"
  - "Environments"
  - "Drupal Development"

# post type
type: "post"
---
As I mentioned in a previous month's article, I'm still using / playing / practicing with DDEV to build development environments in an agile way. And the truth is that the experience couldn't be more nicer. 
We know that for some years, working with Docker Engine has become a MUST. From our ability to define specifications(Dockerfiles), configure autobuilds in the cloud to generate images from our repositories(Dockerhub), run images and containers(Docker), connect containers and relate them(Docker-Composer) or deploy container networks( Docker Swarm) ... on this depends the agility that we can give to our daily work. So I've gathered the most frequent commands in a cheatsheet.

--------------------------------------------------------------------------------------
**This article was originally published  in [https://davidjguru.github.io](https://davidjguru.github.io/blog/containers-docker-docker-compose-ddev-cheatsheet)**    
**Picture from Unsplash, user [Markus Spiske, @markusspiske](https://unsplash.com/@markusspiske)**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[1- Introduction](#1--introduction)  
[2- Basics](#2--basics)  
[3- Dockerfile](#3--dockerfile)  
[4- Images](#4--images)
[5- Containers](#5--containers)  
[6- Docker-Compose](#6--docker-compose)  
[7- Others](#7--others)  
[8- DDEV](#8--ddev)   
[9- :wq!](#9--wq)    
<!-- /TOC -->

-------------------------------------------------------------------------------

## 1- Introduction
Due to the importance of knowing (and practicing) with these processes and daily mechanics, I have thought about gathering the most used commands in my day to day work in the context of Docker.

As a Fast-Cheatsheet of work that compiles the most usual instructions of the different parts of the Docker Engine in the day to day, including DDEV, since for me it is already an inseparable part of both: the Docker Universe and the daily work with projects based on Drupal. 

I have assembled everything by specific blocks (Basics, Docker, Dockerfile, Docker-Composer, Images, Containers and last but not least, my new love DDEV.
And I also took advantage of it to give it a certain didactic approach: some instructions can be summarized or executed more directly, but I think that many colleagues can learn much more when they understand well the logic under which these tools operate.

I would have liked to have included many more things, like DDEV hooks, but this was already on the way to an encyclopedia. I promise to write about it soon.

| ![Picture from Unsplash, user jeshoots.com, @jeshoots](../../images/post/davidjguru_docker_docker_compose_ddev_tools_2.jpg) |
|:--:|
| *Picture from Unsplash, [user JESHOOTS.COM @jeshoots](https://unsplash.com/@jeshoots)* |

## 2- Basics

```
# Uninstall old versions of Docker
sudo apt-get remove docker docker-ce docker-engine \
docker.io containerd runc

# Installing Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
| sudo apt-key add -
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y docker-ce
sudo chmod 666 /var/run/docker*

# Test if Docker is running or not
systemctl is-active docker

# List all the Docker CLI commands
docker

# Get a whole resume about the Docker installation
docker info

# Get the current version installed of Docker in a fast
# and short response 
docker --version

# Get a extended report about the Docker installation 
# with info about client and server
docker version
```

## 3- Dockerfile

```
# Mapping ports in a Dockerfile
ports:
  - "8080:80"
     (Host:Guest)

# Mapping volumes in a Dockerfile
volumes:
  - "local_directory:remote_directory"
volumes:

        - type: volume
          source: /data/mysql
          target: /var/lib/mysql


# Create a tag for a new Dockerfile
docker build . -t vendor/name-tag 

# Remote login to DockerHub
docker login 

# Push the selected Dockerfile tagged to Dockerhub
docker push vendor/tag
```
## 4- Images

```
# Get a list of existing images
docker images // docker image ls

# Get all the images but by its ID
docker image ls -q

# Run an image
docker run hello-world

# Build image from a certain Dockerfile
docker build -t web-custom-name /path/to/dockerfile/

# Run an instance of the former image
# and publish port 8080 in container
# to the port 8282 on host.
docker run -p 8282:8080 web-custom-name

# If the image isn't downloaded, then pull it from 
# remote at DockerHub
docker run centos

# If only want download an image, without run 
# a container
docker pull ubuntu

# Delete images by ID
docker rmi $(docker image ls -q) 

# Remove unused images
docker image prune
```

## 5- Containers

```
# Run a container but in detached mode and
# back to your prompt
docker run -d vendorexample/appexample

# Run a container with  custom name.
docker run -d --name web-custom-name nginx:1.14-alpine

# Restart a Container
docker restart IDCONTAINER

# Run a container from the centOS image with bash and
# login in prompt
docker run -it centos bash

# Deploy a mysql database using the mysql image and
# name it mysql-db. Set the database password to 
# use db_pass123. Lookup the mysql image on 
# Docker Hub and identify the correct environment
# variable to use for setting the root password.
docker run -d -e MYSQL_ROOT_PASSWORD=db_pass123 --name mysql-db mysql

# Run a container in background with a end of life
docker run -d centos sleep 100

# Run a container, mapping ports and mapping volumes and
# using a user from the container
docker run -p 80:8080 -v /locahost/folder:/container/folder \
-u root jenkins/jenkins

# Get a list of existing containers
docker ps

# List all containers showing it by its ID
docker ps -q

# Kill all containers running selected by ID
docker kill $(docker ps -q)

# Remove only a container
docker rm IDCONTAINER

# Remove all containers with status=exited
docker rm $(docker ps -q -f status=exited)

# Executes a command inside a running container
docker exec idcontainer unixcommand 

# Connect to the Prompt of a Container
docker exec -it IDCONTAINER /bin/bash

# Connect to the Prompt of a Container as root
docker exec -ti -u root IDCONTAINER /bin/bash

# Copying files with Docker
# From Local to Remote Docker Container
docker cp db/dump.sql IDCONTAINER:/tmp/dump.sql
# From Remote Docker Container to local
docker cp IDCONTAINER:/tmp/dump_test.sql ./db 

# Attach local standard output, input and error 
# streams to a running container
docker attach IDCONTAINER 

# Inspect all the info about a Docker Container
docker container inspect IDCONTAINER 

# Show the log of a container
docker logs -f IDCONTAINER

# Tailing logs:
# Searching for 'error' (case - insensitive) in the 
# last 1000 log lines of my jenkins (example) 
# container adding the timestamp at the beginning 
# of each line.
sudo docker logs -t --tail 1000 jenkins 2 >&1 \
| grep -i error

# Describe all the existing Docker Compose Networks
docker network ls

# Get all the info about an specific network
docker network inspect name_network 

# Delete a network in your Docker Compose system
docker network rm name_network

# Remove unusued data and clean the Docker System
docker system prune -f

# Show stats about the running containers.
docker stats

# Same but with a formatted output. 
docker stats --all --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Fixing results in prompt.
docker ps -q | xargs  docker stats --no-stream

```
## 6- Docker-Compose

```
# Run a multi-container application with Docker Compose
docker-compose up -d
# Turn on the Docker Compose network
# but building images before starting containers
docker-compose up --build

# Stop a multi-container application with Docker Compose
docker-compose stop

# Kill and delete containers based in Docker Compose
docker-compose down

# Strem the container events for every container 
# in a project
docker-compose events --json 

# Connect to a container using its alias (no ID, no IP)
docker-compose exec ALIAS /bin/bash
# Example: executing drush cache rebuild in a Drupal Container
docker-compose exec web ./vendor/bin/drush cr

# Connecting to a Container (called mysql) as root by prompt
docker-compose exec -u root mysql /bin/bash

# Get the Docker container's log
docker-compose logs -t ALIAS
# Get the Docker container's log 
# with direct connection in real time
docker-compose logs -t -f ALIAS
```

## 7- Others

```
# Docker Swarm: Deploy instances of application 
# across docker host.
docker stack deploy -c docker-compose.yml

# Remove ALL: stopped containers, all networks 
# not used and all dangling images.
docker system prune

# Getting a full summary about the Docker resources
# in your system, ordered by type.
docker system df
```

## 8- DDEV
If you need an introduction to DDEV, I recommend you read this article that I wrote recently (the previous month): [Development environments for Drupal with DDEV](https://www.therussianlullaby.com/blog/creating-development-environments-for-drupal-with-ddev/).

| ![Picture from Unsplash, by @hannahmgibbs](../../images/post/davidjguru_docker_docker_compose_ddev_tools_3.jpg) |
|:--:|
| *Picture from Unsplash, [user Hannah Gibbs, @hannahmgibbs](https://unsplash.com/@hannahmgibbs)* |

```
# Git Clone Project and launch composer install
git clone https://github.com/randomuser/my-drupal8
cd my-drupal8
ddev composer install

# Initial Project 
mkdir my-drupal8
cd my-drupal8
ddev config --project-type php --php-version 7.3
ddev composer create drupal-composer/drupal-project:8.x-dev \
--stability dev --no-interaction
ddev config --project-type drupal8
ddev restart

# Creating a CMS-specific settings file with 
# DDEV credentials pre-populated
ddev config

# DDEV Commons
ddev start
ddev list
ddev describe [project-name]

# Using SSH in DDEV Containers
ddev ssh 

# Executing Drush in DDEV Containers from outside the container.
ddev exec drush status
ddev exec drush cex
ddev exec drush site-install
ddev exec drush site-install standard \
--site-name='Drupal Site Install Test' \
--account-name=admin --account-pass=admin \
--account-mail=mail@example.com -y


# Installing dependencies inside a ddev container.
ddev composer require drupal/devel

# Install a complete Drupal Site using ddev 
# in a "single" instruction.
mkdir NAMEPROJECT && cd NAMEPROJECT \
&& ddev config --project-type php \
--php-version 7.3 \
&& ddev composer create drupal-composer/drupal-project:8.x-dev \
--stability dev --no-interaction && ddev config \
--project-type drupal8 \
&& ddev exec drush site-install standard \
--site-name='NAMEPROJECT' \
--account-name=admin \
--account-pass=admin \
--account-mail=mail@example.com -y \
&& ddev start \
&& sensible-browser http://NAMEPROJECT.ddev.local

# Get a list of projects using DDEV
ddev list

# Describe a project
ddev describe project-name

# Importing a database file (launch a prompt to set 
# the location and values of the database dump)
ddev import-db

# Exporting a database
ddev export-db

# Importing files assets
ddev import-files

# Snapshotting your database
ddev snapshot // Same as in ddev stop --remove-data

# Clean the database and avoid the snapshot
ddev stop --remove-data --omit-snapshot

```

## 9- :wq!

##### Recommended song

{{< youtube r57J0jPyZRs >}}