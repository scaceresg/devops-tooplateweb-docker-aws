# Deployment of a Tooplate Webpage using Docker in AWS

This project consists of deploying a webpage from [Tooplate](https://www.tooplate.com/)
using Docker on an AWS EC2 instance.

## Initial Settings: AWS EC2 Instance

To build and deploy the application, I launched an EC2 instance in AWS with the 
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

## Download Tooplate HTML Template

For this application, I have used Moso Interior template, which can be found in: 
[https://www.tooplate.com/view/2133-moso-interior](https://www.tooplate.com/view/2133-moso-interior)

Inside the EC2 instance, I ran the following commands:

* `sudo apt install wget unzip` to install the dependencies

* `mkdir moso-interior-webpage` to create a directory for the webpage deployment

* `wget https://www.tooplate.com/zip-templates/2133_moso_interior.zip` to get the `.zip`
  file from the Tooplate webpage

* `unzip 2133_moso_interior.zip` to unzip the webpage directory

* Tar the content inside the `2133_moso_interior` directory: `tar czvf moso-interior.tar.gz *`

* Move the file one level up: `mv moso-interior.tar.gz ../` and remove both the `2133_moso_interior` `.zip` file and unzipped directory
