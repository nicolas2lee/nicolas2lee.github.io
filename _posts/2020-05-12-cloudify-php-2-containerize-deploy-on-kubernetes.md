---
layout: post
title:  "Cloudify laravel php REST application 2 - containerize and deploy on cloud"
date:   2020-05-31 10:17:14 +0100
categories: php, docker, kubernetes, helm, cloud
---
It is a guide from A to Z to create a laravel and deploy to cloud under docker, kubernetes technology.

## Build docker image

### Based on official php docker image

### Based on custom php image
For many enterprises, we can not choose any image available in docker hub. So in this case, we need to prepare and install all necessary dependencies.
For example, we use a basic centos image with apache http server installed (centos/httpd-24-centos7), and then we need to provide php runtime environment.
And here comes 2 situations:
#### Install from Internet
We use a third party remi repo to install php, in order to add repo for yum:

    yum --add-repo http://rpms.remirepo.net/enterprise/remi-release-7.rpm

For some security reasons, we may need configure a proxy to download from Internet, then you should configure your yum proxy which is located in /etc/yum.conf:

    proxy=http://<username>:<password>@<proxy>:<port>
    
For some specific symbols in password, like !, @ ..., you should use url encoding to replace them https://www.urlencoder.org/

And then we need to enable this repo:

    yum-config-manager --enable remi-php72
    
Update package list:

    yum update
    
Install necessary php dependencies:
    
    yum install php72 php72-php-fpm php72-php-gd php72-php-json php72-php-mbstring php72-php-xml php72-php-xmlrpc php72-php-opcache php-cli

If we do not install php-cli, then system cannot find php, but php72, in order to make the link, then install php-cli

#### Install locally
In general, for large company environment, for some serious security reason, some internet access is forbidden, then we need to install locally. Then 2 steps should follow:
1. Download necessary yum package
You can do it manually or by using yum downloadonly plugin
    
        yum install --downloadonly --downloaddir=/app php72 php72-php-fpm php72-php-gd php72-php-json php72-php-mbstring php72-php-xml php72-php-xmlrpc php72-php-opcache php-cli

2. Install rpm packages locally

        yum localinstall <path-to-rpm-pacakge>.rpm

A command is quite useful when test with docker image, in order to install package in a docker container, you should oftern need root rightn so this command allow you run docker container as root:

        docker run --user 0 -it <docker-container-id> bash 
        
For alpine version image, you should put bin/ash instead of bash

A complete example:
  
    FROM centos/httpd-24-centos7
    WORKDIR /php-dependencies
    COPY ./rpm-package .
    USER root
    RUN yum -y localinstall **.rpm
    USER default
    CMD /bin/php -v

Based on this php runtime image, then we can deploy application and create our applicative docker image
## Create helm chart
