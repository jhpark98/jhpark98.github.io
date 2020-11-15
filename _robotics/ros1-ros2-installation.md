---
title: "ROS1 & ROS2 Installation on Docker"
excerpt: "Install ROS1 and ROS2 on Docker in your machine!"
header:
  # image: /assets/images/foo-bar-identity.jpg
  # teaser: /assets/images/foo-bar-identity-th.jpg
#sidebar:
#  - title: "Role"
#    image: http://placehold.it/350x250
#    image_alt: "logo"
#    text: "Robotics & A.I. Enthusiast"
#  - title: "Responsibilities"
#    text: "Reuters try PR stupid commenters should isn't a business model"

#gallery:
#  - url: /assets/images/unsplash-gallery-image-1.jpg
#    image_path: assets/images/unsplash-gallery-image-1-th.jpg
#    alt: "placeholder image 1"
---

My Desktop status:
- Ubuntu Version: 18.04.5 LTS (Bionic Beaver)
- Docker Version:

## Installing Docker
Two ways to install Dockers:
- Ubuntu repositories
  - Simple and quick
  1. `$ sudo apt-get update`
  2. `$ sudo apt-get install docker.io`
- Official Docker repository
  - [Official Docker Installation Guide](https://docs.docker.com/engine/install/ubuntu/#installation-methods)
  - I took a recommended approach: set up Docker’s repositories and install from them, for ease of installation and upgrade tasks

  1. Install necessary packages\
  `$ sudo apt-get install apt-transport-https ca-certificates curl
  gnupg-agent software-properties-common`

  2. Add the official GPG key from Docker\
  `$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo
  apt-key add -`

  3. Set up the Docker repository\
  `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`

  4. Update the apt package index once again\
  `$ sudo apt-get update`

  5. Install the Docker package\
  `$ sudo apt install docker-ce`

  6. Check Docker version\
  `$ docker --version

Docker can only be run as a root user by default. Add username to the Docker group.\
`$ sudo usermod -aG docker ${USER}`

### Working with Docker

`$ sudo docker pull ros:melodic-ros-core`
<!-- ```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
``` -->

## Installing ROS Melodic (ROS1 Distribution) on Linux
[ROS Melodic Ubuntu Installation link](http://wiki.ros.org/melodic/Installation/Ubuntu)
* Setup  sources.list
*

## Installing ROS 2 on Linux
[ROS 2 Installation link](https://index.ros.org/doc/ros2/Installation/Crystal/Linux-Install-Binary/)

## Alias of ROS1 and ROS2
