# Installation of Nagios in Ubuntu 20.04

## Table of Containts

  [Linux Distribution](#linux-distribution)

  [Prerequisite tool](#prerequisite-tool)

  [Podman](#podman)

  [Nagios](#nagios)


 # Linux Distribution
 
 **Distribution**: A Linux Distribution refers to a kind of operating system known as Linux.  

Distributor Id - Ubuntu

Version - 20.04

# Prerequisite tool

**Podman version 3.4.2 (This is optional otherwise,you can apply on base as well.)**

**Base**: "Base" means installing and running Nagios directly on your system without using containerization.

# Podman

Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Daemon**: A daemon is a background process or service that runs without direct interaction with the user.

**Daemonless**: Daemonless refers processes that don't run as background 

**Before Install Podman check OS version (If version is 20.04 or Earlier then add repository)**

Check OS Version -
```
cat /etc/os-release
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ cat /etc/os-release
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https\://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```
**Version**: is like a specific edition of software or a product. It often represents a set of features or changes. Like Ubuntu version-20.04, 22.04.

**Release**:when a specific version of software is made officially available to the public.

**cat**-cat stands for "concatenate" and Its primary purpose is to concatenate and display the content of files.

**/etc/os-release**- Os-release is a file located in the /etc directory on Linux systems.

**Add Repository-**
Podman is not in the default Ubuntu repository, we need to add the repository to be able to download the podman package. 

**Repository**: A"repository" is a place where things are stored and organised , like files, code, or other project-related items.

**Install the Podman dependencies**:
```
sudo apt install software-properties-common uidmap
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo apt install software-properties-common uidmap
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  python3-software-properties software-properties-gtk
The following NEW packages will be installed:
  uidmap
The following packages will be upgraded:
  python3-software-properties software-properties-common
  software-properties-gtk
3 upgraded, 1 newly installed, 0 to remove and 270 not upgraded.
Need to get 128 kB of archives.
After this operation, 172 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://us.archive.ubuntu.com/ubuntu focal-updates/main amd64 software-properties-common all 0.99.9.12 [10.4 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu focal-updates/main amd64 software-properties-gtk all 0.99.9.12 [69.1 kB]
Get:3 http://us.archive.ubuntu.com/ubuntu focal-updates/main amd64 python3-software-properties all 0.99.9.12 [21.7 kB]
Get:4 http://us.archive.ubuntu.com/ubuntu focal-updates/universe amd64 uidmap amd64 1:4.8.1-1ubuntu5.20.04.4 [26.4 kB]
Fetched 128 kB in 12s (10.5 kB/s)                                              
(Reading database ... 143669 files and directories currently installed.)
Preparing to unpack .../software-properties-common_0.99.9.12_all.deb ...
Unpacking software-properties-common (0.99.9.12) over (0.99.9.11) ...
Preparing to unpack .../software-properties-gtk_0.99.9.12_all.deb ...
Unpacking software-properties-gtk (0.99.9.12) over (0.99.9.11) ...
Preparing to unpack .../python3-software-properties_0.99.9.12_all.deb ...
Unpacking python3-software-properties (0.99.9.12) over (0.99.9.11) ...
Selecting previously unselected package uidmap.
Preparing to unpack .../uidmap_1%3a4.8.1-1ubuntu5.20.04.4_amd64.deb ...
Unpacking uidmap (1:4.8.1-1ubuntu5.20.04.4) ...
Setting up uidmap (1:4.8.1-1ubuntu5.20.04.4) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for libglib2.0-0:amd64 (2.64.6-1~ubuntu20.04.4) ...
Processing triggers for man-db (2.9.1-1) ...
```

**sudo**: This part of the command is like saying "I want to do something important." It
stands for "superuser do".

**apt**: It is used to handle the installation, removal, and upgrading of software packages.

 **install**: This tells the magic tool that you want to install a new program on your
computer.

**software-properties-common**:It provides common tools for managing software repositories.

**uidmap**: It likely provides functionality related to user ID.

**Adding repository**
```
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/ /' > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list"
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo sh -c "echo 'deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/ /' > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list"
vivek@vivek-HP-EliteBook-840-G2:~$
```
**sudo:** This part of the command is like saying "I want to do something important." It
stands for "superuser do".

**sh -c**:This command creates a new shell. The -c option specifies that the command written after it should be executed.

**echo**: This command used to print the text.

**deb**: Indicates that this is a binary package repository.

**http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/**: This is the URL of the repository, pointing to the  Kubic libcontainers stable packages for Ubuntu 20.04.

**/etc/apt/sources.list.d/**: This is directory where The repository being added.

**devel:kubic:libcontainers:stable.list**: This is the name of file.


**And Run the sudo apt-get update**
```
sudo apt update
```
**If you get an error like this following on updating time**:
```
W: GPG error: http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04 InRelease: The following signatures couldn’t be verified because the public key is not available: NO_PUBKEY 4D64390375060A
```
This error message indicates that the GPG key is missing.Then need to import the missing GPG key for the repository.
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4D64390375060A(replace with your key)
```
**Output**:
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4D64390375060AA4
Executing: /tmp/apt-key-gpghome.K1X2ZwrFrU/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4D64390375060AA4
gpg: key 4D64390375060AA4: public key "devel:kubic OBS Project <devel:kubic@build.opensuse.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
madhur@madhur-Standard-PC-Q35-ICH9-2009:~$ sudo apt-get update
Hit:1 http://us.archive.ubuntu.com/ubuntu focal InRelease                      
Hit:2 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease
Hit:3 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease
Err:4 http://security.ubuntu.com/ubuntu focal-security InRelease            
  Temporary failure resolving 'security.ubuntu.com'
Get:5 http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04  InRelease [1,642 B]
Get:6 http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04  Packages [15.0 kB]
Fetched 16.6 kB in 18s (937 B/s)    
Reading package lists... Done
```
**sudo**: This part of the command is like saying "I want to do something important." It
stands for "superuser do".

**apt-key**: This command is used to manage the keys by apt to authenticate packages.

**adv**: This is an option used with apt-key for advanced options.

**--keyserver hkp**://keyserver.ubuntu.com:80: This specifies the key server to use for retrieving the GPG key.

**--recv-keys**: This option tells apt-key to fetch the GPG key. (replace with id like 4D64390375060A)

**Update system:-**
```
sudo apt update
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo apt update
[sudo] password for vivek: 
Hit:1 https://dl.google.com/linux/chrome/deb stable InRelease
Hit:2 http://ppa.launchpad.net/ondrej/php/ubuntu focal InRelease               
Hit:3 http://security.ubuntu.com/ubuntu focal-security InRelease               
Hit:4 http://archive.ubuntu.com/ubuntu focal InRelease                         
Get:5 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]        
Get:6 https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04  InRelease [1,642 B]
Hit:7 http://archive.ubuntu.com/ubuntu focal-backports InRelease      
Fetched 115 kB in 2s (47.6 kB/s)
Reading package lists... Done
Building dependency tree       
Reading state information... Done
1 package can be upgraded. Run 'apt list --upgradable' to see it.
vivek@vivek-HP-EliteBook-840-G2:~$ 
```
**sudo:** This part of the command is like saying "I want to do something important." It
stands for "superuser do".

**apt:** It is used to handle the installation, removal, and upgrading of software packages. 

**update:** It means, check the internet to see if there are any new or updated programs available.

**Podman Install:-**
```
sudo apt install podman
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo apt install podman
[sudo] password for vivek:
Reading package lists... Done
Building dependency tree   
Reading state information... Done
podman is already the newest version (100:3.4.2-5).
0 upgraded, 0 newly installed, 0 to remove and 9 not upgraded.
```
**sudo**: This part of the command is like saying "I want to do something important." It
stands for "superuser do".

**apt**: It is used to handle the installation, removal, and upgrading of software packages.

 **install**: This tells the magic tool that you want to install a new program on your
computer.


**Check podman :-**
```
which podman
```
**Output**:
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ which podman
/usr/bin/podman 
vivek\@vivek-HP-EliteBook-840-G2:\~$
```
-**Which :** Which command is used to locate the full path of the executable file.

# Nagios
Nagios is a monitoring system used for keeping track of the health and performance of computer systems, networks, and infrastructure components. It helps administrators and IT professionals by providing real-time insights into the status of various resources.

 
**Why Nagios**:
Nagios is an open-source monitoring tool that monitors networks, servers, and applications. Its popularity stems from its flexibility, real-time alerts, and capability to store historical data.

**Functionality**:
* Detect all files of network or server issues.
* It help you to find the root cause of the problem which allow you to get the permanent solution to the problem.
* Reduce downtime.
* Active monitering of entire infrainstructure.
* It allow you to moniter and troubleshoot server performance issue.

**Feature**:
* Oldest and latest
* Nagios autometic send alert if condition change like- to change condition from normal to critical, warning,pending.
* You can moniter entire the aplication process with a single features.
* It use by default port 5666,5667,5668 to moniter its client.


**Now search the Nagios image-**
```
podman search nagios
```

**podman:** It is the name of the containerization tool.

**search:** It is a subcommand in a podman tool, it indicates that you want to search for container image images in container registry.

**nagios:** I am looking for container images related to the Nagios monitoring tool.

**This comand is used to pull the nagios container image from docker hub to your local system using podman**
```
podman pull docker.io/jasonrivers/nagios
```
**podman pull:** It tells your computer to download something.

**docker.io/jasonrivers/nagios:** This is the name of image.

This command will start the Nagios container with the specified options. You can access the Nagios web interface by opening a web browser and navigating to http:localhost:8080 and open By default username and password “nagiosadmin”
```
podman run -p 8080:80 --cap-add=NET_RAW -v /home/vivek/rough/nagios:/usr/local/nagios/etc/ --name nagios -d docker.io/jasonrivers/nagios
```
**podman run**: This is the command to run a container using Podman.

**-p 8080:80**: This option maps port 8080 on the host to port 80 on the container.

**--cap-add=NET_RAW**: This command is like giving a special power to the container. This power allows the container to do advanced things with computer networks.

**-v /home/vivek/rough/nagios**:/usr/local/nagios/etc/: This option mounts the local directory.

**--name nagios**: This option assigns the name "nagios" to the running container.

**-d**: This option runs the container in detached mode.

**docker.io/jasonrivers/nagios**: This is the name of the Docker image used to create the container. 

Now check the running container this command-
```
podman ps
```
**Output**:
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman ps

CONTAINER ID  IMAGE                                 COMMAND           CREATED   STATUS        PORTS                                       NAMES

0f41ef7d6d11  docker.io/jasonrivers/nagios:latest   /usr/local/bin/st...  37 hours ago  Up 8 seconds ago  0.0.0.0:8080->80/tcp                        nagios
```
**podman**: this command is used for managing containers and container images.

**ps**: ps command is short for "process status." When used with podman, it shows the status of containers.

Now you can access the Nagios web interface by opening a web browser and navigating to http:localhost:8080 and **open by default username “nagiosadmin” and password "nagios".**

Now access the web page <http://localhost:8080>

![](https://github.com/vivekpandey1222/Documentation/assets/151363790/a5adc9ab-e8c1-4db9-94e9-b652c68380e6)

After login you will get Nagios dashboard like this:

![Screenshot from 2024-01-08 23-10-51](https://github.com/vivekpandey1222/Documentation/assets/151363790/01759b6d-3dcd-489f-8762-48087c5cc127)



**Checking services**<a id="checking-services"></a>

We have to go to the left side and click on the service option. This is the dashboard of all services.

![](https://github.com/vivekpandey1222/Documentation/assets/151363790/47c7abe9-1648-4202-9941-419042079084)

