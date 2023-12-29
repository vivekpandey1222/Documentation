# Installation of Nagios in Ubuntu 20.04

## Table of Containts

  [Linux Distribution](#linux-distribution)

  [Prerequisite tool](#prerequisite-tool)

  [Podman](#podman)

  [Nagios](#nagios)


 # Linux Distribution
 
 **Distribution**: A Linux Distribution refers to a kind of operating system known as Linux. It is a packaged combination that includes the Linux kernel  along with various essential programs and tools. 

Distributor Id - Ubuntu

Version - 20.04

# Prerequisite tool

**Podman version 3.4.2 (This is optional otherwise,you can apply on base as well.)**

**Base**: "Base" refers to the basic state of your computer. When we say "apply some software or application to base," we are talking about the initial state or existing settings as they appear on your local machine. I mentioned about nagios"Which tells you that you can use podman or apply it to your own computer.

# Podman

Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless alternative to Docker. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Daemon**: A daemon is like a helpful, invisible assistant that works in the background on your computer.

**Daemonless**: Daemonless means doing things without relying on such invisible assistants. It suggests a setup where tasks are managed without continuous background processes.

**Before Install Podman check OS version (If version is 20.04 then add repository)**

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

**Release**:when a specific version of software or a product is made officially available to the public.

**cat**-cat stands for "concatenate" and Its primary purpose is to concatenate and display the content of files.

**/etc/os-release**- Os-release is a file located in the /etc directory on Linux systems.

**Add Repository-**
Podman is not in the default Ubuntu repository, we need to add the Kubic repository to be able to download the podman package. 

**Repository**: A"repository" is a place where things are stored and organised , like files, code, or other project-related items.

**Adding repository Ubuntu-**
```
echo  "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu\_${VERSION\_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
```
**echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /"**: echo is a command that simply prints the specified text to the standard output.

**sudo**: This command allows a permitted user to execute a command as the superuser.

**tee**: This is a command that reads from standard input and writes to standard output and files.

**/etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list**: This is the path to the file where the repository entry will be saved. 

 **Check curl if installed then no need to install again**-
 
**What is the need to use curl**?

It is not mandatory to use curl in podman but curl is a command-line tool that  you exchange data between your device and a server through a command-line interface (CLI).

**Install curl**

```
sudo snap install curl
```
**snap**: Snap is a package management system and application packaging format developed by Canonical for Linux distributions. It allows applications to be bundled with their dependencies and run in a containerized environment called a snap.

**install**: This is the subcommand used with snap to indicate that you want to install a specific application or package.

**curl**: This is the name of the package (in this case, the curl tool) that you want to install. 

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
stands for "superuser do" and allows you to perform tasks that affect your computer's
system, like installing software.

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
**- sudo:** This part of the command is like saying "I want to do something important." It
stands for "superuser do" and allows you to perform tasks that affect your computer's
system, like installing software.

**- apt:** It is used to handle the installation, removal, and upgrading of software packages.

 **- install:** This tells the magic tool that you want to put a new program on your
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
Nagios is an open-source monitoring tool that monitors networks, servers, and applications. Its popularity stems from its flexibility, real-time alerts, and capability to store historical data. Nagios provides the freedom to customize and aids in taking proactive measures.

**Functionality**:
* Detect all files of network or server issues.
* It help you to find the root cause of the problem which allow you to get the permanent solution to the problem.
* Reduce downtime.
* Active monitering of entire infrainstructure.
* It allow you to moniter and troubleshoot server performance issue.

**Feature**:
* Oldest and latest
* Good log and autometic send alert if condition change like- to change condition from normal to critical, warning,pending.
* You can moniter entire the aplication process with a single features.
* Moniter network service like- Http,ssh,smtp.
* It use by default port 5666,5667,5668 to moniter its client.


**Now search the Nagios image-**
```
podman search nagios
```

**podman:** It is the name of the containerization tool.

**search:** It is a subcommand in a podman tool, it indicates that you want to search for container image images in container registry.

**nagios:** It is search term or query, here I am looking for container images related to the Nagios monitoring tool.
This comand is used to pull the nagios container image from docker hub to your local system using podman.
```
podman pull docker.io/jasonrivers/nagios
```
**podman pull:** It tells your computer to download something.

**docker.io:** It's the location on the internet where we want to get something.

**jasonrivers/nagios:** This specifies exactly what we want to download, in this case, it's a tool or application called "nagios."

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

![](https://lh7-us.googleusercontent.com/tdeH1HzXnOLcEErgYQTKrMVJvl7b85WHNt4yczO6fRd3vMYHoqqiWqrD-hQ76tuXhwSdtku7F8NsAtUnBJSMgawZAyIUFEOuLaZq-e0tXAD_4leTH-hWn5a4h8pfkwe4OHU-mNSxJls2Zt5pjM1GsVQ)


**Checking services**<a id="checking-services"></a>

We have to go to the left side and click on the service option. This is the dashboard of all services.

![](https://github.com/vivekpandey1222/Documentation/assets/151363790/47c7abe9-1648-4202-9941-419042079084)

