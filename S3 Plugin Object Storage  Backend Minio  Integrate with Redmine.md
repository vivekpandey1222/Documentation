## S3 Plugin Object Storage Backend Minio Integrate with Redmine
## Table of Content

[Minio](#1minio)

[Minio-Testing](#2minio-testing)

[Redmine-Setup](#3redmine-setup)

[s3-plugins-integration](#4s3-plugins-integration)

[testing-and-test-case](#5testing-and-test-case)

[generate-yaml-file](#6generate-yaml-file)

[launch-pod-using-yaml](#7launch-pod-using-yaml)


**Linux Distribution-**

Distributor Id - Ubuntu

Version - 20.04

**Podman :-**

Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless alternative to Docker. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Update system:-**
```
sudo apt update 
```
**Podman Install:-**
```
sudo apt install podman 
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ sudo apt install podman
[sudo] password for vivek:
Reading package lists... Done
Building dependency tree   
Reading state information... Done
podman is already the newest version (100:3.4.2-5).
0 upgraded, 0 newly installed, 0 to remove and 9 not upgraded.

**- sudo:** This part of the command is like saying "I want to do something important." It
stands for "superuser do" and allows you to perform tasks that affect your computer's
system, like installing software.

**- apt:** It is used to handle the installation, removal, and upgrading of software packages.

- **install:** This tells the magic tool that you want to put a new program on your
computer.

**Check podman :-**
```
which podman 
```

vivek\@vivek-HP-EliteBook-840-G2:\~$ which podman

/usr/bin/podman 

vivek\@vivek-HP-EliteBook-840-G2:\~$

-**Which :** Which command is used to locate the full path of the executable file.

—----------------------------------------------------------------------------------------------------------------------------

## 1.Minio:-

Minio is an open-source object storage server that is compatible with S3. Minio is the best server which is suited for storing unstructured data, like photos, videos,  backups, and more. 

**Setup Minio:-**

vim pod-minio.sh

vivek\@vivek-HP-EliteBook-840-G2:\~$ cat pod-minio.sh
```
#!/bin/bash

podman run -dt -p 9001:9000 -e MINIO_ROOT_USER=xxxxx -e MINIO_ROOT_PASSWORD=xxxxxxxx --name minio -v /home/vivek/minio:/data docker.io/minio/minio server /data --console-address ":9001"
```
`-podman run: `This command is used to run a container using Podman.

`-dt: `The -d flag runs the container in the background (detached mode), and the -t  flag allocates a pseudo-TTY.

`-p 9001:`This option maps port 9001 on the host to port 9000 in the container. This is where the Minio server will be accessible on the host machine.

`-e MINIO_ROOT_USER=xxxxx -e MINIO_ROOT_PASSWORD=xxxxxxxx`**:** These options set environment variables within the container. They are configuring the Minio server with a root user and password.

`--name minio`**:** This option sets the name of the container to "minio."

`-v /home/vivek/minio:/data`**:** This option mounts the local directory `/home/vivek/minio` into the container at the path /data. This is typically used to persist data outside the container.

`docker.io/minio/minio`**:** This is the name of the Minio image on Docker Hub.

`server /data`**:** This is the command that will be executed inside the container. It tells Minio to start a server using the data directory /data.

`--console-address ":9001`**:** This option specifies the address on which the Minio console will be available. it's set to port 9001.

**Chmod +x ,This command  used to change file permissions.**
```
chmod +x pod-minio.sh:-
```
**Once the execute permission is granted, you can run the script** 
```
./pod-minio.sh
```
**Then create container and running container command** 
```
podman ps
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman ps
CONTAINER ID  IMAGE                                     COMMAND               CREATED       STATUS                PORTS                                           NAMES
7bf686d7d295  docker.io/minio/minio:latest              server /data --co...  3 weeks ago   Up About an hour ago  0.0.0.0:9000-9001->9000-9001/tcp                minio
```
After running the script, open your web browser and check the Minio server <http://localhost:9001>

—----------------------------------------------------------------------------------------------------------------------------

## 2.Minio Testing:-

Testing Minio object storage  bucket using ansiblePlaybook

**install ansible:-**
```
sudo apt install ansible
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo apt install ansible
[sudo] password for vivek:
Reading package lists... Done
Building dependency tree
Reading state information... Done
ansible is already the newest version (2.9.6+dfsg-1).
O upgraded, 0 newly installed, 0 to remove and 5 not upgraded.
vivek@vivek-HP-EliteBook-840-G2:~$
```

**-Ansible:**Ansible is an open-source automation tool that helps you automate repetitive tasks. It allows you to define and manage your infrastructure code.

**Check Ansible:-**
```
which ansible
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ which ansible /usr/bin/ansible
vivek@vivek-HP-EliteBook-840-G2:~$
```
**Using playbook create object Minio bucket:-**

**Create directory:-**
```
touch minio-testing.txt
```
**Then create yml file :-**
```
vim minio-testing.yml
```

```
---
- hosts: localhost
connection: local
gather_facts: 
  tasks:

- name: Simple PUT operation (Upload-Single file)

   aws_s3:
     aws_access_key: xxxxx # Replace with your Minio access key
     aws_secret_key: xxxxxxxx # Replace with your Minio secret key
     bucket: project1
     object: minio_testing.txt
     src: minio_testing.txt
     mode: put
     s3_url: http://127.0.0.1:9001
     region: us-east-1
     encrypt: false
   vars:

          # ansible_python_interpreter: /usr/bin/python3
```

**Then run command for testing :-**
```
ansible-playbook minio-testing.yml -v
```
![](https://lh7-us.googleusercontent.com/sjYeoLji9K-LpKeuSt0l8qK_0KwYxdMXIaoUlYHMUiO4zzLQfUZhH622YGnFfwaaK5B6fbvqUrtR0pf3WmaaJovjPuisiq7l_Yv26dvN9jo74TTI3C5Drd9ADH1_Ffgeq8STse_dMbOJSDNlBjTlDeE)

**Here you can see my minio_testing.txt file** 

![](https://lh7-us.googleusercontent.com/4NsRKGPOTe28FqJU_drov5e9fZfhR2_QepfePS_Lq_L50--oeh_Ztcmqo2_Nwmg7h-7kObfa4zmCSU6DT5bvY_vlZ5oxzEP7ZoH8b767MDidP3PyFcQBXME61vh6gnSOsZyMSp-dKPZDI48kOsMfAgk)

**-ansible-playbook:** This command is used to run Ansible playbooks. 

**-playbook:** A playbook is a file written in YAML format.

**-Yaml:** It is easy for both humans and machines to read.

**-minio-testing.yml:** This is the name of the Ansible playbook file that Ansible will execute. 

**-v:** It is used with Ansible, it increases the verbosity. Increasing verbosity means providing more information about what the tool is doing during its execution.

Note:- If an error while running this command, (ansible-playbook minio-testing.yml -v)
Then check python3  library
```
pip install boto3
```
![](https://lh7-us.googleusercontent.com/fTEBh4CwHD27YB77UQEbTv06w2221q2nknt-k-pEJgX8h3NUuC_uOQt0NrhRvclpMUpNR_hFV0bsIURbcuBuiJl2MS3jFNl509jbx1j2fA7KCy8KiI0pV11Lveice--AkB8dn3-DnkpKLo1PJE-m00E)

**-pip:** It is used to install and manage Python packages.

**-boto3:** Boto3 is the official Amazon Web Services Software Development Kit for Python. It allows Python developers to write software that makes use of services like Amazon S3 

—----------------------------------------------------------------------------------------------------------------------------

## 3.Redmine Setup:-

Redmine is a free and open source, web-based project management and issue tracking tool. It allows users to manage multiple projects and subprojects. 

**Postgres:-**

PostgreSQL is a powerful open-source relational database management system (RDBMS) that performs a variety of tasks related to storing, organizing, and retrieving structured data. 

**Redmine Setup:-**
```
vim redmine.sh

vivek\@vivek-HP-EliteBook-840-G2:\~$ cat redmine.sh

#!/bin/bash

podman pod create --name postgres-redmine --publish 3000:3000 --publish 5432:5432

podman run -dt \
--pod postgres-redmine \
--name redmine-postgres-intigration \
-e POSTGRES_DB=redminedb \
-e POSTGRES_USER=xxxxx \
-e POSTGRES_PASSWORD=xxxxx \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v /home/vivek/redmine/postgres-uat:/var/lib/postgresql/data \
postgres

podman run -dt \
--pod postgres-redmine \
--name redmine-app-new \
-e REDMINE_DB_PORT=5432 \
-e REDMINE_DB_USERNAME=xxxxxx \
-e REDMINE_DB_PASSWORD=xxxxxx \
-e REDMINE_DB_DATABASE=redminedb \
-e AWS_REGION=us-east-1 \
-e AWS_ACCESS_KEY_ID= \
-e AWS_SECRET_ACCESS_KEY= \
redmine
```
**Chmod +x ,command is used to add execute permission to a fi**le
```
chmod +x redmine.sh
```
**Once the execute permission is granted, you can run the script** 
```
./redmine.sh
```
**Then create container and running container command** 
```
podman ps
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman ps
```
After running the script, open your web browser and check the Minio server <http://localhost:3000>

And sign in by default username and password “admin”.
Then create your own username and password.

—----------------------------------------------------------------------------------------------------------------------------

## 4.S3 plugins Integration:-

Amazon S3, (Simple Storage Service) is a widely used object storage service provided by Amazon Web Services. It enables users to store and retrieve any amount of data at any time over the internet.

**S3 Plugins Install on redmine**

Root directory to redmine
```
podman exec -it \<container name> /bin/bash
```
```
vivek@vivek-HP-EliteBook-840-G2:-$ podman exec it redmine-app-new /bin/bash 
root@postgres-redmine:/usr/src/redmine#
```
**-podman exec :** This command for executing  within a running container.

**--it:** It is commonly used for running an interactive shell inside the container.

**-container name:** This is a place for the name or ID of the container.

**-/bin/bash:** This command will be executed inside the specified container.

**Now run this command on redmine container**
```
git clone https\://github.com/redmica/redmica\_s3.git plugins/redmica\_s3
```
**-git clone:** This is the Git command used to clone a repository. Cloning is the process of creating a local copy of a Git repository on your machine.

**-https\://github.com/redmica/redmica\_s3.git :** This is the URL of the Git repository.

**-plugins/redmica\_s3:** This is the local directory where the repository will be cloned.



**Then run this command and edit config/s3.yml with your favorite editor** 
```
cp plugins/redmica\_s3/config/s3.yml.example config/s3.yml
```
```
vim config/s3.yml 

root\@postgres-redmine:/usr/src/redmine# cat config/s3.yml

production:

  access_key_id: admin key
  secret_access_key: xxxxxxxxx
  bucket: redmine-data
  folder: case-attachment
  endpoint: http://192.168.122.1:9000
  thumb_folder:
  import_folder:
  region: us-east-1


development:

  access_key_id:
  secret_access_key:
  bucket:
  folder:
  endpoint:
  thumb_folder:
  import_folder:
  region:

# Uncomment this to run plugin tests with standart redmine script
# test:
#   access_key_id:
#   secret_access_key:
#   bucket:
```
Now download dependency

bundle install 
```
root\@postgres-redmine:/usr/src/redmine# bundle install
```
**-bundle:** Bundle is a tool used in Ruby programming to keep track of and manage the different pieces of code, called "gems," that a project depends on. 

Then run this command 

rake redmine:plugins
```
root\@postgres-redmine:/usr/src/redmine#rake redmine:plugins
```
```
root@postgres-redmine:/usr/src/redmine/config# rake redmine:plugins (in /usr/src/redmine)
root@postgres-redmine:/usr/src/redmine/config#
root@postgres-redmine:/usr/src/redmine/config#
```
**-Rake redmine:plugins:** is a Rake task designed for managing plugins in a Redmine installation.

Now you can see the plugins in redmine : 

Click on the administration option and click on the plugins options .

![](https://lh7-us.googleusercontent.com/4T-tqjRr8-2iEd0MMKIzVHzuR3pyE82U3s6zUYWk--JiV1ZmcGVm1EpaS2IvKhg1ITMJsiywLtnoWFFryxRDXluXW2n3n5L5HjrbyykNhhFrZwEtJeOr4fSv20FqZotyhqgS9tRTRUTzvgyIONwRjys)

**Restart your pod and server:-**
```
podman pod restart postgres-redmine
```
```
podman ps
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman pod restart postgres-redmine 
9d33b0c6d766af131f72070d5c51506daa2464ca6dc1e0e98e6ac1c6b3338ec0
vivek@vivek-HP-EliteBook-840-G2:~$ podman pod ps
POD ID        NAME              STATUS      CREATED       INFRA ID      # OF CONTAINERS
9d33b0c6d766  postgres-redmine  Running     19 hours ago  0573a099e29e  3
vivek@vivek-HP-EliteBook-840-G2:~$ 
```


**Create image of container**

commit command is used to create image of container 
```
podman commit redmine-app-new localhost/redmine-app-new-pandey-v1
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman commit redmine-app-new localhost/redmi ne-app-new-pandey-v1
Getting image source signatures
Copying blob cb4596cc1454 skipped: already exists
Copying blob 59c86fca9a73 skipped: already exists
Copying blob 18fcaa610edd skipped: already exists
Copying blob 4c87f2fc892e skipped: already exists
Copying blob d4249a43d6b4 skipped: already exists
Copying blob 91b1944a286a skipped: already exists
Copying blob bec9e66a32f9 skipped: already exists
Copying blob a24f8ecf754e skipped: already exists Copying blob 2d4bd63f282f skipped: already exists
Copying blob c0662c7b9fb3 skipped: already exists
Copying blob 1c37a547bf9d skipped: already exists
Copying blob b1ced07e6fd4 skipped: already exists
Copying blob 1c26ae039210
done Copying config 3554759189 done
Writing manifest to image destination
Storing signatures
3554759189d2c256c472922754641ef3d90a9ebe7c07ab3c3960c4cccdf77514
vivek@vivek-HP-EliteBook-840-G2:~$
```
**Search container image**
```
podman images
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman images
REPOSITORY                             TAG          IMAGE ID      CREATED       SIZE
localhost/redmine-app-newvivek-v1      latest       b90a40dd470c  2 days ago    784 MB
```
—----------------------------------------------------------------------------------------------------------------------------

## 5.Testing and test case :-

**Creating Ticket:-** 

Login your own username and password.

![](https://lh7-us.googleusercontent.com/iG4PJIwv9NPRAXAdFgdG0-aPPNK9EZFDZy_EdieU4KsjtQjnKX3_CLVkQHl5oUL-uru3QDi-_XB4VsAOvMLthnIbxbgivKgGzTrffFv6AYaBe7ptvF_6p3oUDu_CuZzK9zzMbDbq0MRVCtfPVI0cmEk)

Then click on project options and click on the right side “new project”

![](https://lh7-us.googleusercontent.com/QhLJMtiQPS0StUV6AYGFsaOV0uBMUUfvsDbpsRzLgpVILfH8Bx2v_1QwQJM2Y3uq3iBtnY3wQwE7WAe2CBMWgGbKEIM0Gt3reBE_SQ_mxV1i5tyCQJkfZ_2pjLwwXqzKqRQyS4gywLJZ8qGxtR64GBE)

And create a project eg-testminio,

![](https://lh7-us.googleusercontent.com/E6Qllq-Qh2PPZgfEmXjxy2-2BbG9xKuTSA7zbpCgyrcL8ej8IS_AIu8_i0Ohb_wHmsutr6n7jxdEg1X6hLsMe1uLwOsPyoODQi3xmm0Pot2_GiOL0AudCjdv9UghoqnPDA2-1_XTxo689IWd81K8pYc)

Then click on the administration and manage the redmine project and issue settings .

![](https://lh7-us.googleusercontent.com/ZecV_82n0sxdBvrLDR9P3VLp4iVcI_xiW7eEHTv4jY-ed_pqKAWqbbU5IUwoGa0hP_2peHpdu5AQ4pXvq5XK-C4ShfNNd_xFO2CGfLL5nFA9r6VvG1jXZtrNDqk5LKU7iCaKJ2zTFDCWGbdXUb-V-1Q)

After Configure the redmine setting click on the project and go to issue options and click on new issue options .

![](https://lh7-us.googleusercontent.com/kOGlitqfPR6rTHeygePoKLYQAHtFSC4fkhu0CVawADwHh2oIyWdAdBau4VZLMsoL8ATJfn9GgSFNOTqvDFxfm1YPZFRgvUsgozxJbDC0eX-ZvzL35tkr_Zv3BHy1Kt4O4HLqnwR9Cv1GYBicEheJllQ)

Create new project task 

![](https://lh7-us.googleusercontent.com/ZTSjAynJ3E6MiQ4yt9zvhb2Z0vyHvCS9buZJ2cRkQuIJ6iC0EfBMaDqeToXLTzLf_nGKRz0PiMntZHvUjtAoMDnDw5eGccUlM3xYlwQtK_MLbDS4jOT72eRvwLjCatIKkwZf-YDM2zsjEKkstiyJAQw)

**Artefact #1**

Upload file while create project task ,

Now here you can see the upload file “abc.txt”

![](https://lh7-us.googleusercontent.com/NEB4MIxnw0W3bgkKERICTxEzqMKBpLXWrvs8Umxa9ULJ4asuJtpNGYbRnmFKGU3xzuZdBeyFyuGJJpc6fDW2ec2bX1A5c7II8u54A16zArr6aQBpO6xdgLgAFN-bpcpZEareTtFh4M007IsVfDDqnYE)

**Artefact #2**

Here you can see all data has been shifted to Object storage

![](https://lh7-us.googleusercontent.com/MxufXcY_MuPk1Prp13Wvr_A7oH1UopNCE241s_DHUYRnivgl7v12a8aS3KgsFbMtyeuz9lbsibnm4RZqLm5j0Tl1IfGkVeV7a6fjuLGYKMhDo_K2qHnJHJ3mRgQ_Xz1XPyt5R4_TVfi2pK4GBvkNfEM)

**Artefact #3**

Data Has been deleted from local folder now local folder is blank
```
podman exec -it postgres-redmine /bin/bash
```
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman exec it redmine-app-new /bin/bash
root@postgres-redmine:/usr/src/redmine#
```
```
 ls and cd files 
```
```
root@postgres-redmine:/usr/src/redmine# ls
CONTRIBUTING.md  app	       db     log	    sqlite
Gemfile		 appveyor.yml  doc    package.json  test
Gemfile.lock	 bin	       extra  plugins	    tmp
README.rdoc	 config        files  public	    vendor
Rakefile	 config.ru     lib    redminedb     yarn.lock
root@postgres-redmine:/usr/src/redmine# cd files/
root@postgres-redmine:/usr/src/redmine/files# ls
root@postgres-redmine:/usr/src/redmine/files# 
```

**Artefact #4**

Able to download data from same ticket

![](https://lh7-us.googleusercontent.com/amlRwJsTo00kCYRtNlcXQ6NpgFPwaya0T3iPO_NW6KiyoLpLFap23qcgfKzeAOBPC2vbmVRRpIL5_qibww1AgCYI4QCRddlE3khlbtXeqFkahaxUvl47PK1kkn8_yBL4YCEKdloUeQ3K7rstmb7kf_A)

—----------------------------------------------------------------------------------------------------------------------------

## 6.Generate Yaml File:-

Run this command to generate a yaml file.
```
podman generate kube \<pod name> > \<File Name>.yaml
```
**-podman generate kube:** This command is used to generate a Kubernetes YAML manifest file for a specified container or pod. 

**-\<pod name>:** This is the name of your pod.

**->:** It takes the output of the podman generate kube command and writes it to a file.

**-\<File Name>.yaml:** Here enter the name of the file you want to create.

—---------------------------------------------------------------------------------------------------------------------------.

## 7.Launch Pod using yaml:-
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman generate kube postgres-redmine > kube-postgres-redmine.yaml
```
**Now you can see here yaml file eg-   kube-postgres-redmine.yaml:-**
```
**vivek\@vivek-HP-EliteBook-840-G2:\~$** cat kube-postgres-redmine.yaml

# Save the output of this file and use kubectl create -f to import
# it into Kubernetes.
# Created with podman-3.4.2
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2023-11-21T06:42:18Z"
  labels:
app: postgres-redmine
  name: postgres-redmine
spec:
  containers:
  - args:
- postgres
image: docker.io/library/postgres:latest
name: redmine-postgres-intigration
env:
- name: POSTGRES_DB
   value: redminedb
- name: POSTGRE_USER
   name: redmineuser
- name: POSTGRES_PASSWORD
   value: redmine123
- name: PGDATA
   value: /var/lib/postgresql/data/pgdata
ports:
- containerPort: 3000
   hostPort: 3000
- containerPort: 5432
   hostPort: 5432
resources: {}
securityContext:
   capabilities:
     drop:
     - CAP_MKNOD
     - CAP_NET_RAW
     - CAP_AUDIT_WRITE
tty: true
volumeMounts:
- mountPath: /var/lib/postgresql/data
   name: home-vivek-redmine-postgres-uat-host-0
  - args:
- rails
- server
- -b
- 0.0.0.0
image: docker.io/library/redmine:latest
name: redmine-app-new
env:
- name: REDMINE_DB_PORT
   value: 5432
- name: REDMINE_DB_USERNAME
   value: redmineuser
- name: REDMINE_DB_PASSWORD
   value: redmine123
- name: REDMINE_DB_DATABASE
   value: redminedb
- name: AWS_REGION
   value: us-east-1
- name: AWS_ACCESS_KEY_ID
   value: xxxxxxxxxxx
- name: AWS_SECRET_ACCESS_KEY
   value: xxxxxxxxxx
resources: {}
securityContext:
   capabilities:
     drop:
     - CAP_MKNOD
     - CAP_NET_RAW
     - CAP_AUDIT_WRITE
tty: true
volumeMounts:
- mountPath: /usr/src/redmine/files
   name: 3c5d6664579752606d0f4675bdd7ed049c0996ef1c159c10c7cb381daf9cc5c1-pvc
  restartPolicy: Never
  volumes:
  - hostPath:
   path: /home/vivek/redmine/postgres-uat
   type: Directory
name: home-vivek-redmine-postgres-uat-host-0
  - name: 3c5d6664579752606d0f4675bdd7ed049c0996ef1c159c10c7cb381daf9cc5c1-pvc
persistentVolumeClaim:
   claimName: 3c5d6664579752606d0f4675bdd7ed049c0996ef1c159c10c7cb381daf9cc5c1
status: {}
```
**vivek\@vivek-HP-EliteBook-840-G2**

**Remove pod :**
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman pod rm postgres-redmine --force
```
3d03764227139c340b7dabb6cca395930fb9d998f939652ce122b6bb569327cc

**Now Create pod using yaml command:**

**podman play kube kube-podtgres-redmine.yaml**
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman play kube kube-postgres-redmine.yaml
```
```
Trying to pull docker.io/library/postgres:latest...
Getting image source signatures
Copying blob b66a9305e34c done  
Copying blob 969823a73455 done  
Copying blob 799c670608dc done  
Copying blob 870cc573d452 done  
Copying blob 3cf63429c214 done  
Copying blob 1f7ce2fa46ab done  
Copying blob fb2a58049417 done  
Copying blob 0829252d0d9a done  
Copying blob a797f18545f0 done  
Copying blob 79e525202743 done  
Copying blob a3104899b78b done  
Copying blob 43ef4fb89a17 done  
Copying blob bbf10b4ac81d done  
Copying config 2167863c43 done  

Writing manifest to image destination
Storing signatures
Pod:
9d33b0c6d766af131f72070d5c51506daa2464ca6dc1e0e98e6ac1c6b3338ec0
Containers:
a18e224c9b830799ad236f4ae50b7af077edc6eacffc07440c0f7ae57577f5da
f2d8f00f0bc3df153916cbadaf39c9f60a2cf01080a50b80dea87f28d704fce0
vivek\@vivek-HP-EliteBook-840-G2:\~$
```
-**podman play kube:**This command is used  for deploying and managing containers in a Kubernetes cluster using Podman. 

**-kube-postgres-redmine.yaml:** This is the name of the Kubernetes YAML manifest file that you want to deploy.
