**Installation of Nagios in Ubuntu 20.04**

## Table of Content

[1.Linux Ddistriution](#1linux-distribution)

[2.Prerequisites-tool](#2prerequisites-tool)

[3.podman](#3podman)

[4.nagios](#4nagios)

### 1.Linux Distribution-

Distributor Id - Ubuntu

Version - 20.04

### 2.Prerequisites tool-

Podman version 3.4.2 (This is optional otherwise,you can apply on base as well.)

### 3.Podman :-**

Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless alternative to Docker. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Before Install Podman check OS version (If version is 20.04 then add repository)**

Check OS Release -
```
cat /etc/os-release
```
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
**Add Repository-**

Podman is not in the default Ubuntu repository, we need to add the Kubic repository to be able to download the podman package. 

**Adding repository Ubuntu-**
```
echo  "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu\_${VERSION\_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
```

**Check curl if installed then no need to install again -**

**Install curl-**
```
sudo snap install curl
```
Then, use the command below to download and add the GPG key. This is needed to make sure the downloaded package is good.
```
curl -L "https://download.opensuse.org/repositories/devel:/kubic:/libcontainers\:/stable/xUbuntu\_${VERSION\_ID}/Release.key" | sudo apt-key add -
```

**Update system:-**
```
sudo apt update
```
**Podman Install:-**
```
sudo apt install podman
```
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

 **install:** This tells the magic tool that you want to put a new program on your
computer.


**Check podman :-**
```
which podman
```
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ which podman
/usr/bin/podman 
vivek\@vivek-HP-EliteBook-840-G2:\~$
```
-**Which :** Which command is used to locate the full path of the executable file.

### 4.Nagios:-
Nagios is a monitoring system used for keeping track of the health and performance of computer systems, networks, and infrastructure components. It helps administrators and IT professionals by providing real-time insights into the status of various resources.

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
podman run -p 8080:80 -v /home/vivek/rough/nagios:/usr/local/nagios/etc/ --name nagios -d docker.io/jasonrivers/nagios
```
Now check the running container this command-
```
podman ps
```
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman ps

CONTAINER ID  IMAGE                                 COMMAND           CREATED   STATUS        PORTS                                       NAMES

0f41ef7d6d11  docker.io/jasonrivers/nagios:latest   /usr/local/bin/st...  37 hours ago  Up 8 seconds ago  0.0.0.0:8080->80/tcp                        nagios
```
Now access the web page <http://localhost:8080>

Now login with by default username and password "nagiosadmin"

![](https://github.com/vivekpandey1222/Documentation/assets/151363790/a5adc9ab-e8c1-4db9-94e9-b652c68380e6)

After login you will get Nagios dashboard like this:

![](https://lh7-us.googleusercontent.com/tdeH1HzXnOLcEErgYQTKrMVJvl7b85WHNt4yczO6fRd3vMYHoqqiWqrD-hQ76tuXhwSdtku7F8NsAtUnBJSMgawZAyIUFEOuLaZq-e0tXAD_4leTH-hWn5a4h8pfkwe4OHU-mNSxJls2Zt5pjM1GsVQ)


**Checking services**<a id="checking-services"></a>

We have to go to the left side and click on the service option. This is the dashboard of all services.

![](https://lh7-us.googleusercontent.com/7N1KAxVJWGkC7RoZdF9bw36ZYmyfoGpAKYvLMyHZb21jllvE85M3swpI213pxCw7yKyhNMtimfCvXvBwf4pX69LAU-PA3RoPI-RRQd8uPSKX2UhY9Mt0t1V0CIEPTQImcW_a7XY4NJMoROHPpOnJvPY)
