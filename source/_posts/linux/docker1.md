---
title: "docker(1): docker installation on ubuntu 14.04 "
date: 2016-08-09 09:30
---

> Install Docker on ubuntu

###### Update your apt sources

      1. $ sudo apt-get update
      2. $ sudo apt-get install apt-transport-https ca-certificates
      3. $ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
      4. Open the /etc/apt/sources.list.d/docker.list file in your favorite editor.
      5. On Ubuntu Trusty 14.04 (LTS)   deb https://apt.dockerproject.org/repo ubuntu-trusty main
      6. $ sudo apt-get update

###### Install


      7. $ sudo apt-get purge lxc-docker
      8. $ sudo apt-get install docker-engine
      9. $ sudo service docker start
      10. $ sudo docker run hello-world

> Configurations

###### Create a Docker group

###### Adjust memory and swap accounting

###### Enable UFW forwarding

###### Configure a DNS server for use by Docker

###### Configure Docker to start on boot

> Upgrade Docker

    $ sudo apt-get upgrade docker-engine

> Uninstallation

    $ sudo apt-get purge docker-engine
    $ sudo apt-get autoremove --purge docker-engine
    $ rm -rf /var/lib/docker



[官方文档](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
