---
layout: post
title:  "Linux Setup"
---

In this post, we are going to take a look into useful Linux programs and how to
install them and see what is really going behind the curtain.

## Install Atom Text Editor
1. Update the packages list and install the dependencies
```
$ sudo apt update
$ sudo apt install software-properties-common apt-transport-https wget
```
2. Import the Atom Editor GPG key with wget command
```
$ wget -q https://packagecloud.io/AtomEditor/atom/gpgkey -O- | sudo apt-key add -
```
Enable the Atom APT repository
```
$ sudo add-apt-repository "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main"
```
3. Install the latest version of Atom
```
$ sudo apt install atom
```

## Starting Atom
### Debian / Ubuntu (apt-get)
1. Open the terminal and install Git using apt-get
```
$ sudo apt-get update && sudo apt-get install git
```
2. Configure Git username and email
```
$ git config --global user.name "<Your Name>"
$ git config --global user.email "<Your Email>"
# For Example:
$ git config --global user.name "Ji Hwan Park"
$ git config --global user.email "jamesparkjihwan73@gmail.com"
```
## Installing Vim
### What is `vim`?
-
1. Update package database
```
$ sudo apt update
```
2. Install Vim package
```
$ sudo apt install vim
```
3. Verify Vim version
```
$ vim --version
```
## Installing gedit
### What is `gedit`?
-https://help.ubuntu.com/community/gedit
1. Update package database
```
$ sudo apt update
```
2. Install Vim package
```
$ sudo apt-get install gedit
```
3. Verify Vim version
```
$ vim --version
```
### What is the difference between `apt` vs `apt-get`?
- https://itsfoss.com/apt-vs-apt-get-difference/
"----------------------------------------"
## How do you install packages in Ubuntu?

### Check Version
```
# '-m': print the machine hardware name
$ uname -m
>>> x86_64
```

### What is the role of `sudo apt-get update`?
### `apt-get update` vs. `apt-get upgrade`
