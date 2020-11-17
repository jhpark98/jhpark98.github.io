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
- ROS Version: Melodic Morenia
- ROS2 Version: Dashing Diademata

## Installing Docker
Two ways to install Dockers:
- Ubuntu repositories
  - Simple and quick
1. `$ sudo apt-get update`
2. `$ sudo apt-get install docker.io`
- Official Docker repository
  - [Official Docker Installation Guide](https://docs.docker.com/engine/install/ubuntu/#installation-methods)
  - I took a <u>recommended approach<u>: set up Docker’s repositories and
  install from them, for ease of installation and upgrade tasks.

  1. Install necessary packages\
  `$ sudo apt-get install apt-transport-https ca-certificates curl
  gnupg-agent software-properties-common`

  2. Add the official GPG key from Docker\
  `$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo
  apt-key add -`\
It will return: `OK`

  3. Set up the Docker repository\
  `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`\
It will return: `Reading package lists... Done`

  4. Update the apt package index once again\
  `$ sudo apt-get update`

  5. Install the Docker package\
  `$ sudo apt install docker-ce`

  6. Check Docker version\
  `$ docker --version`

Docker can only be run as a <u>root user<u> by default. Make sure to add username to the Docker group.\

I suggest two ways to make your life easier in the future.
1. create the docker group and add your user:
  1. Create the `docker` group.
  `$ sudo groupadd docker`
  2. Add your user to the `docker` group.
  `$ sudo usermod -aG docker ${USER}`

2. Sideways
```
# Wihtout `sudo`, we see an error(?):
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock ...
# This allows us to connect to the Docker daemon socket without accessing as a root user.
sudo chmod 666 /var/run/docker.sock
# From now on, you can omit `sudo` in your command.
```

p.s `chmod` is an abbreviation of *change mode.* `chmod` command changes attributes from a file/folder. `chmod 666 file/folder` enables <u>all users<u> to read and write, but cannot execute the file/folder.

## ROS1(Melodic) Working with Docker
1. Pull a ROS Image\
`$ sudo docker pull ros:melodic-ros-core`
  - In our case, our image is created from `ros` repository with a `melodic-ros-core` tag.
2. View docker iamges\
  `$ sudo docker images`
  - With `docker images` command, we get the list of (docker) images.
3. Run Docker Container\
  - Derive container with: `docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`
  - `$ sudo docker run -it ros:melodic-ros-core`
  -`-it` is a shorter version of `-i -t`. Since we want to move into an interactive session, we add that option in command to allocate a tty for the container process
  - Long story short, it allows us to be in terminal as if we're in a new bash terminal separate from host machine. (basically, bash for container)

- (in interactive session)
  - If you put `pwd`, it returns current working directory, which is `/`
  - Move into `root` folder using: `cd root`
  - From now on, we create a folder on top of this root folder.
  - Once you are in the interactive session, you no longer have to use `sudo`
  - Basically, you can just do `apt-get update`
- 3-1. Open new bash for the same container
  - We can start additional bash session in the same container; in the future, you do this way to start a talker for one bash and start a listener for another bash.
  - `docker exec -it <container name> bash`
  - or `docker exec -it <container ID> bash`

4.  Commit the modifications
  - `$ sudo docker commit <container-id> ros:melodic-ros-core`



### Setting up the ROS workspace
1. Create an empty workspace folder and a `src` folder and switch into `src` folder.
- `$ mkdir -p catkin_ws/src`
- `$ cd ~/catkin_ws/src`

2. Initialize catkin workspace with following command:
- `$ catkin_init_workspace`

3. Using [catkin_make](http://wiki.ros.org/catkin/commands/catkin_make) to build workspace
- At `catkin_ws` folder, use `catkin_make` command to build the workspace.
- Use `ls` command to check `build` and `devel` folders are created.

4. Add the workspace environment to our `.bashrc` file
- `$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc`
- `$ vim ~/.bashrc` to check we are sourcing appropriately
- `$ source ~/.bashrc`

5. Verify everything is good to go
- `$ echo $ROS_PACKAGE_PATH`
- If two locations are source, we are good to go.

We are now good to move on to setting up ROS2 package.

## Installing ROS-2
### Building ROS 2 Dashing Diademata on Linux
- [Building ROS 2 on Linux](https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Development-Setup/)
- We will build ROS2 manually on Linux, which is called source installation.

1. Set locale
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

2. Add the ROS 2 apt repository
- Add the ROS 2 apt repositories to our system:
- `sudo apt update && sudo apt install curl gnupg2 lsb-release`
- `curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

- Add the repository to the sources list:
- `sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'`

3. Install development tools and ROS tools:
```bash
# Notice that we are soon going to use these tools that we are installing
$ sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  python3-colcon-common-extensions \
  python3-pip \
  python-rosdep \
  python3-vcstool \
  wget
# install some pip packages needed for testing
$ python3 -m pip install -U \
  argcomplete \
  flake8 \
  flake8-blind-except \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order \
  flake8-quotes \
  pytest-repeat \
  pytest-rerunfailures \
  pytest \
  pytest-cov \
  pytest-runner \
  setuptools
# install Fast-RTPS dependencies
$ sudo apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev
# install Cyclone DDS dependencies
$ sudo apt install --no-install-recommends -y \
  libcunit1-dev
```

4. Getting the ROS-2 source code
```bash
Create `ros2_ws` and `src` folders
# Create parent directory `ros2_ws` and its subdirectoriy `src`
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws
# `wget` allows you to download files from web. In this case, we download the latest ROS2 version(clone all repos)
$ wget https://raw.githubusercontent.com/ros2/ros2/release-latest/ros2.repos
Import the ROS2 repository(`ros2.repos`) to `src` folder
$ vcs import src < ros2.repos
```
Since the workspace is all set, we move on to install dependencies.

### Installing dependencies using rosdep
```bash
$ sudo rosdep init
$ rosdep update
$ rosdep install --from-paths src --ignore-src --rosdistro dashing -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 rti-connext-dds-5.3.1 urdfdom_headers"
```

### Build the code in the workspace
- Similar to what we had in ROS1(`catkin_make` command in `catkin_ws` folder), in ROS2, we use `colcon build`
```bash
cd ~/ros2_ws/
colcon build --symlink-install
```

## Setting up ROS-1, ROS-2, or both environments
### Setup Environment
- Use the `alias` command and add them to the bash script:
1. Invoke bash script
- `gedit` is the text editor of the GNOME desktop environment. Use following command to install:
```
$ sudo apt update && sudo apt upgrade && sudo apt-get install gedit
$ sudo gedit ~/.bashrc
# if `getdit` fails to work, just use `vim ` editor to edit the script
$ sudo vim ~/.bashrc
```
2. Add two lines of `alias` command
- `$ alias initros1='source /opt/ros/melodic/setup.bash'`
- `$ alias initros2='source ~/ros2_ws/install/local_setup.bash'`
- If you have created workspace with different name , change `ros2_ws` to `my_workspace`
- `$ alias initros2='source ~/my_workspace/install/local_setup.bash'`
- `$ cd install && ls` to check that there is `local_setup.bash`

3. Delete or comment following lines that we have wrote in `~/.bashrc` previously
- `$ source /opt/ros/melodic/setup.bash`
- `$ source ~/catkin_ws/devel/setup.bash`

4. Save and close the bash script, open up new Termina, and source bash file to use the `alias` command
- `$ source ~/.bashrc`
- `initros1` will allows us to work with ROS1 packages.
- `initros2` will allows us to work with ROS2 packages.

### Running test nodes
1. Open a Terminal and source the ROS-2 environment workspace
- `$ initros2`

2. Run talker node
- `$ ros2 run demo_nodes_cpp talker`
- Notice that `rosrun` in ROS-1 changed to `ros2 run` in ROS-2.

3.  In another Terminal, initialize ROS-2 and run listener node.
- $ initros2
- $ ros2 run demo_nodes_py listener

4. See the list of topics
- `$ ros2 topic list`

### Setting up the ROS-2 workspace
- Let's consider the `ros2_examples_ws` package for demonstration purposes:
```bash
$ initros2
$ mkdir ~/ros2_workspace_ws && cd ~/ros2_workspace_ws
$ git clone https://github.com/ros2/examples src/examples
$ cd ~/ros2_workspace_ws/src/examples

"# check out the branch that is compatible with our ROS distro version, that is, dashing"
$ git checkout dashing
$ cd ..
$ colcon build --symlink-install

" run tests for the packages we built"
- `$ colcon test`
"Now, you can run the package nodes after sourcing the package environment:"
- `$ . install/setup.bash`
- `$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function`

"Open new Terminal"
```
$ initros2 # source ROS2 environment
$ cd ~/ros2_workspace_ws
$ . install/setup.bash # source the package environment
$ ros2 run examples_rclcpp_minimal_publisher publisher_member_function
```

```
"----------------------------------------"
### Installing ROS Melodic (ROS1 Distribution) on Linux
[ROS Melodic Ubuntu Installation link](http://wiki.ros.org/melodic/Installation/Ubuntu)
1. Configure  Ubuntu repositories
  - Hit `Software Updater` → `Settings` → `Ubuntu Software` → Check to allow "restricted," "universe," and "multiverse."
2. Setup sources.list
  - `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

3. Set up keys
  - `sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`

4. Installation
- As usual, check Debian package index is up-to-date\
- `sudo apt update`\
- In my case, I am going to install the recommended Desktop-Full install one.\
- `sudo apt install ros-melodic-desktop-full`

5. Environment setup
- If you miss this step, then you are going to have to explicitly source `source /opt/ros/melodic/setup.bash` everytime you open up a new shell.
- `echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc`\
- Whenever we open up our shell, all we have to do is:\
- `source ~/.bashrc`
- Since we are going to install both ROS1 and ROS2, there is going to be some changes made in our `~/.bashrc` script. So, watch out!

6. Dependencies for building packages
- Up to this point, we have only installed what we need to run the **core ROS packages**.
- To efficiently create and manage our ROS workspaces, we install some dependencies:
- `sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential`
- 6.1. Initialize rosdep
  - Initialize rosdep with following commands:
    - `sudo rosdep init`
    - `rosdep update`

### Installing ROS 2 on Linux (Local)
- Same steps as Installing ROS-2 above.
- [ROS 2 Installation link](https://index.ros.org/doc/ros2/Installation/Crystal/Linux-Install-Binary/)
