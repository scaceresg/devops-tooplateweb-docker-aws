# Containerisation of a Tooplate Webpage using Docker in AWS

This project consists of containerising a webpage from 
[Tooplate](https://www.tooplate.com/) using Docker on an AWS EC2 instance.

## Initial Settings: AWS EC2 Instance

To build the webpage template, I launched an EC2 instance in AWS with the 
following specifications:

* Name: `build-tooplateweb-docker`

* AMI: `Ubuntu Server 24.04 LTS`

* Type: `t2.micro`

* RSA Key-pair generated as a `.pem` file

* Security group: Inbound rule

  - Type: All traffic

  - Source: My IP

I ran `sudo apt update` once inside the EC2 instance and checked the specifications 
using `cat /etc/os-release`:

```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

## Install Docker Engine

I followed the steps to install Docker Engine for Ubuntu, as mentioned in the 
[Docker documentation page](https://docs.docker.com/engine/install/), using the 
`apt` repository.

## Prepare the Tooplate HTML Template

For this application, I have used Moso Interior template, which can be found in: 
[https://www.tooplate.com/view/2133-moso-interior](https://www.tooplate.com/view/2133-moso-interior)

Inside the EC2 instance, I ran the following commands:

* `sudo apt install wget unzip` to install the dependencies

* `mkdir moso-interior-webpage` to create a directory for the webpage deployment

* `wget https://www.tooplate.com/zip-templates/2133_moso_interior.zip` to get 
the `.zip` file from the Tooplate webpage

* `unzip 2133_moso_interior.zip` to unzip the webpage directory

* Tar the content inside the `2133_moso_interior` directory: 
`tar czvf moso-interior.tar.gz *`

* Move the file to the deployment directory: 
`mv moso-interior.tar.gz ../moso-interior-webpage` and remove both the
 `2133_moso_interior` `.zip` file and unzipped directory

## Prepare the `Dockerfile`

I prepared a `Dockerfile` using `vim Dockerfile` inside the `moso-interior-webpage/`
directory.

The `Dockerfile` contains the instructions to containerise the webpage:

```
FROM ubuntu:latest

LABEL "Author"="scaceresg"
LABEL "Project"="moso-interior"

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www/html

RUN apt update && apt install apache2 git -y

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

ADD moso-interior.tar.gz /var/www/html

EXPOSE 80

VOLUME /var/log/apache2
```

## Build the Docker Image

The final step is to build the image and run the container using the `Dockerfile`:

* Build the image: `docker build -t moso-webpage-img:v1`

* Check the image using `docker images`:

```
REPOSITORY         TAG       IMAGE ID       CREATED          SIZE
moso-webpage-img   v1        9b38480e626c   11 seconds ago   259MB
```

* Run the container: `docker run -d --name moso-webpage -p 8080:80 moso-webpage-img:v1`

* Check the container using `docker ps`

```
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                   NAMES
639c5bd17df6   moso-webpage-img:v1   "/usr/sbin/apache2ct…"   17 seconds ago   Up 17 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   moso-webpage
```

* The webpage is shown in the EC2 instance public IPv4: e.g., `54.162.21.102:8080`