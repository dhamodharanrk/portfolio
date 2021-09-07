---
title: Conda Installation steps on Linux
tags: [Python,Linux]
style: border
color: info
description: "Steps to install conda on linux"
---

This tutorial will guide to Install Anaconda on an Ubuntu.

###### 1. Get Latest Version of Anaconda official website 1. Get Latest Version of Anaconda official website
 
 ```python
https://www.anaconda.com/distribution/
```
Find the latest Linux version and copy the installer bash script

###### 2. Download the Anaconda Bash Script

Logged into your Ubuntu 18.04 server as a sudo non-root user, move into the /tmp directory and use curl to download the link you copied from the Anaconda website:

```python
cd /tmp
curl -O https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh

```

###### 3. Verify the Data Integrity of the Installer

Ensure the integrity of the installer with cryptographic hash verification through SHA-256 checksum:

```python
sha256sum Anaconda3-2019.03-Linux-x86_64.sh
```
and it retruns,
```python
45c851b7497cc14d5ca060064394569f724b67d9b5f98a926ed49b834a6bb73a  Anaconda3-2019.03-Linux-x86_64.sh
```

###### 4. Run the Anaconda Script

```python
bash Anaconda3-2019.03-Linux-x86_64.sh
```

You’ll receive the following output to review the license agreement by pressing ENTER until you reach the end.

```shell
Output

Welcome to Anaconda3 2019.03

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>
...
Do you approve the license terms? [yes|no]
```
When you get to the end of the license, type yes as long as you agree to the license to complete installation.

###### 5. Complete Installation Process

Once you agree to the license, you will be prompted to choose the location of the installation. You can press ENTER to accept the default location, or specify a different location.

```shell
Output
Anaconda3 will now be installed into this location:
/home/sammy/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/sammy/anaconda3] >>
```

At this point, the installation will proceed. Note that the installation process takes some time.

###### 6. Select Options

Once installation is complete, you’ll receive the following output:

```shell
Output
...
installation finished.
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/sammy/.bashrc ? [yes|no]
[no] >>>
```

It is recommended that you type yes to use the conda command.

###### 7. Activate Installation

You can now activate the installation with the following command:

```shell
source ~/.bashrc
```

[Read More](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart "Read More")
