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
- Docker Version: 19.03.13

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
  - It will return: `OK`

  3. Set up the Docker repository\
  `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`
  - It will return: `Reading package lists... Done`

  4. Update the apt package index once again\
  `$ sudo apt-get update`

  5. Install the Docker package\
  `$ sudo apt install docker-ce`

  6. Check Docker version\
  `$ docker --version`

Docker can only be run as a root user by default. Make sure to add username to the Docker group.\
`$ sudo usermod -aG docker ${USER}`

### Working with Docker
- Pull a ROS Image\
`$ sudo docker pull ros:melodic-ros-core`
  - In our case, our image is created from `ros` repository with `melodic-ros-core` tag.
  - Add `sudo` before docker command to make sure you don't get permission error
- View docker iamges\
  `$ sudo docker images`
  - With `docker images` command, we get to see the list of images.

## Installing ROS Melodic (ROS1 Distribution) on Linuxs
[ROS Melodic Ubuntu Installation link](http://wiki.ros.org/melodic/Installation/Ubuntu)
0. Configure  Ubuntu repositories
  - Hit `Software Updater` → `Settings` → `Ubuntu Software` → Check to allow "restricted," "universe," and "multiverse."
1. Setup sources.list\
  - `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

2. Set up keys\
  - `sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`

3. Installation\
- As usual, check Debian package index is up-to-date\
- `sudo apt update`\
- In my case, I am going to install the recommended Desktop-Full install one.\
- `sudo apt install ros-melodic-desktop-full`

4. Environment setup\
- If you miss this step, then you are going to have to explicitly source `source /opt/ros/melodic/setup.bash` everytime you open up a new shell.
- `echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc`\
- Whenever we open up our shell, all we have to do is:\
- `source ~/.bashrc`
- Since we are going to install both ROS1 and ROS2, there is going to be some changes made in our `~/.bashrc` script. So, watch out!

5. Dependencies for building packages\
- Up to this point, we have only installed what we need to run the **core ROS packages**.
- To efficiently create and manage our ROS workspaces, we install some dependencies:
- `sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential`
- 5.1. Initialize rosdep
  - Initialize rosdep with following commands:
    - `sudo rosdep init`
    - `rosdep update`

## Installing ROS 2 on Linux
[ROS 2 Installation link](https://index.ros.org/doc/ros2/Installation/Crystal/Linux-Install-Binary/)

## Alias of ROS1 and ROS2
