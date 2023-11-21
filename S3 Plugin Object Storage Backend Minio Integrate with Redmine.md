**S3 Plugin Object Storage Backend Minio Integrate with Redmine**

**Linux Distribution-**

Distributor Id - Ubuntu

Version - 20.04

**Podman :-**

Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless alternative to Docker. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Update system:-**
```
Sudo apt update 
```
**Podman Install:-**

- Sudo apt install podman 

![](https://lh7-us.googleusercontent.com/TARAvIEZ8EREo6XEmw-Ahy17GKXRPWN-j8_1oBfMGfZPZsYiAXbcISFX7eU5cBfheNQp6-1aEtzG9RkDQfmbbIHJvRnSpI7leF5B2LLOm6OON5C0QZF7bWzvV_9VaIv31OrmKxyYb91fsYJ7djcGBRA)

**Check podman :-**

- Which podman 

![](https://lh7-us.googleusercontent.com/b6gnCkoUDXCvbU62FCMt_XYVAznXS2gA0rpGPc0OcmfxJ_0u3vbEaC9UUTxjMa5H5tfr5Ih9zVKNGQNEM_fffU6DAgOK1lQHt3oQQTaO_EehyBsguXQ9yBTEkXa-brxOGXx_tBzbo9ynxvfKx7fwZDs)

**1.Minio:-**

Minio is an open-source object storage server that is compatible with S3. Minio is the best server which is suited for storing unstructured data, like photos, videos,  backups, and more. 

**Setup Minio:-**

- Vim pod-minio.sh

vivek\@vivek-HP-EliteBook-840-G2:\~$ cat pod-minio.sh

\#!/bin/bash

podman run -dt -p 9001:9000 -e MINIO\_ROOT\_USER=xxxxx -e MINIO\_ROOT\_PASSWORD=xxxxxxxx --name minio -v /home/vivek/minio:/data 

docker.io/minio/minio server /data --console-address ":9001"

**Chmod +x ,command is used to add execute permission to a fi**le

- Chmod +x pod-minio.sh:-

**Once the execute permission is granted, you can run the script** 

- ./pod-minio.sh

**Then create container and running container command** 

- Podman ps

![](https://lh7-us.googleusercontent.com/zLSRcIxpbmem9yBWK-7FwZQyTAV_oBr6dGY4cesq4Hu9UbT6F7CNqqlm9pIs_31kJEvcOVIxGc9cDfinl-xpKQSHcVnblX007-n_T9r32qB9ZSl7hzrXhFsoVEigXEj7q1x-s-ggUa4ZoEcVdvb2qeg)

After running the script, open your web browser and check the Minio server <http://localhost:9001>

\


**2.Minio Testing:-**

Testing Minio object storage  bucket using ansiblePlaybook

**Install Ansible:-**

- Sudo apt install Ansible

![](https://lh7-us.googleusercontent.com/K9pR98bTWI3CQoH3Lg0kpznLMXpKQF9hfu6uSMqjxvbBJvDltoClOsUH_fYIh4aNb6l-kZ1zVI_ESB_m4XQPgKNgvZxn-ODesleDcuZgfGe27rxoZ7ezZpGdC0jArEzyCVrEjC1w0Ls5jOlID6IW22w)

**Check Ansible:-**

- Which ansible

![](https://lh7-us.googleusercontent.com/fbC_sjpNLXOSMvudbUIjbigc2GPOETexheIG_eHlCNJ_ZrvDtUgrvlUT2Ltmov7sUpbq_SYKDvwub9rfEfDwtW6aYe9AAjKMjJmQ1KSC7pQH094m40Ywrqn2X4cYIULmR5k_gXePuIkyvWBk6m3bQgM)

**Using playbook create object Minio bucket:-**

**Create directory:-**

- Touch minio-testing.txt

**Then create yml file :-**

- Vim minio-testing.yml

\---

\- hosts: localhost

  connection: local

  gather\_facts: false

  tasks:

\- name: Simple PUT operation (Upload-Single file)

   aws\_s3:

     aws\_access\_key: xxxxx # Replace with your Minio access key

     aws\_secret\_key: xxxxxxxx # Replace with your Minio secret key

     bucket: project1

     object: minio\_testing.txt

     src: minio\_testing.txt

     mode: put

     s3\_url: http\://127.0.0.1:9001

     region: us-east-1

     encrypt: false

   vars:

           # ansible\_python\_interpreter: /usr/bin/python3

**Then run command for testing :-**

- ansible-playbook minio-testing.yml -v

![](https://lh7-us.googleusercontent.com/b5UyCMz77rMnZZg0TEEoHrirVqEkc1rRIzAgs4j-f4mHsvcd09962Qh4a3Bey7ahcnzlDDldjB_HqH366dSa--ve3sZFulP9ZMebn9ZyE9AHZaeGZQeM1A-8ehrOtMQAFcC6to7Agx3MZt3mdmk7lPk)

**Here you can see my minio\_testing.txt file** 

![](https://lh7-us.googleusercontent.com/QE5oRXQVYwy6xgljrC8k6qKsk1VlnIOTUB3TrWY6XQ2JVpCJS6uNLABeeeJutfmqm82t5Pqsz1he8VltTWXdQ0S28qkcnRnskxnE0lqBSPkk_YTMbBs4FRRcfHNtSdp6XFE5kdNkTMGkOnmJYnHcLTk)

Note:- If an error while running this command, (ansible-playbook minio-testing.yml -v)

Then check python3 and python library

- sudo apt install python3

![](https://lh7-us.googleusercontent.com/h9lei8irOIcyqbSBpDeBdvSOub4bebwmw8AhO5NwkqhkPmzbfQAuLW7YLWVBMRdUGMVXhDueSBfFNlGhEaGjXPaVbtAsnMQmUf3gJ2LQJg-Rd2_QVhY8ybEIz_2Gq-KYBPj48W8ufyriDtoVhuBjH6I)

- Pip install boto3

![](https://lh7-us.googleusercontent.com/sAGpgRwL0w7pwezpE-pKdXJ-ZG0-cnFh6ARdoN2pbDa_KMXwK8QneQlnmphhCETOKy0VduMTdaBADsnQcwDetgLVXe29s9i9bpCzt7CiCIN2dcaXDDlLKaMOF4fk8li551uZkc4QWT3LBT3sr25xlC4)

**3. Redmine:-**

Redmine is a free and open source, web-based project management and issue tracking tool. It allows users to manage multiple projects and subprojects. 

**Postgres:-**

PostgreSQL is a powerful open-source relational database management system (RDBMS) that performs a variety of tasks related to storing, organizing, and retrieving structured data. 

**Redmine Setup:-**

- Vim redmine.sh

vivek\@vivek-HP-EliteBook-840-G2:\~$ cat redmine.sh

podman pod create --name postgres-redmine --publish 3000:3000 --publish 5432:5432

podman run -dt \\

\--pod postgres-redmine \\

\--name redmine-postgres-intigration \\

-e POSTGRES\_DB=redminedb \\

-e POSTGRES\_USER=xxxxx \\

-e POSTGRES\_PASSWORD=xxxxx \\

-e PGDATA=/var/lib/postgresql/data/pgdata \\

-v /home/vivek/redmine/postgres-uat:/var/lib/postgresql/data \\

postgres

podman run -dt \\

\--pod postgres-redmine \\

\--name redmine-app-new \\

-e REDMINE\_DB\_PORT=5432 \\

-e REDMINE\_DB\_USERNAME=xxxxxx \\

-e REDMINE\_DB\_PASSWORD=xxxxxx \\

-e REDMINE\_DB\_DATABASE=redminedb \\

-e AWS\_REGION=us-east-1 \\

-e AWS\_ACCESS\_KEY\_ID= 

-e AWS\_SECRET\_ACCESS\_KEY=

redmine

**Chmod +x ,command is used to add execute permission to a fi**le

- Chmod +x redmine.sh

**Once the execute permission is granted, you can run the script** 

- ./redmine.sh

**Then create container and running container command** 

- Podman ps

![](https://lh7-us.googleusercontent.com/reKd8b_bwefpONb_xVq3v7sc1d3CTY_1VLIUk9gQ5J-cRRdf0uuQyOM803yM7NBkScRS0DpX7Kpt7Hv9QcVrPr4EtIZaPvQNfPGb8raYEnkvYcn6T49yyPqVkaoqRrmjml46otcLnl2Yigom7Xq0zyM)

After running the script, open your web browser and check the Minio server <http://localhost:3000>

And sign in by default username and password “admin”.

Then create your own username and password.

**4.S3 plugins Integration:-**

Amazon S3, (Simple Storage Service) is a widely used object storage service provided by Amazon Web Services. It enables users to store and retrieve any amount of data at any time over the internet.

**S3 Plugins Install on redmine**

Root directory to redmine

- Podman exec -it \<container name> /bin/bash

![](https://lh7-us.googleusercontent.com/dslyEBTYQ4o9md7v41sLeEyHLucZKMtd0ibldl-h3qDhIX2HYXdazS2T3_-cxL8Am0n153f77yVyo6SEsJamLKXn637ljGqddp2tgqiVu6ksRpduD03zWK1EUj4vbvWCA6PpHgqKQv3hb6NmkWMiuqk)

Now run this command on redmine container

- git clone https\://github.com/redmica/redmica\_s3.git plugins/redmica\_s3

Then run this command and edit config/s3.yml with your favorite editor 

- cp plugins/redmica\_s3/config/s3.yml.example config/s3.yml
- Vim config/s3.yml 

\


root\@postgres-redmine:/usr/src/redmine# cat config/s3.yml

production:

  access\_key\_id: admin key

  secret\_access\_key: xxxxxxxxx

  bucket: redmine-data

  folder: case-attachment

  endpoint: http\://192.168.122.1:9000

  thumb\_folder:

  import\_folder:

  region: us-east-1

\
\
\


development:

  access\_key\_id:

  secret\_access\_key:

  bucket:

  folder:

  endpoint:

  thumb\_folder:

  import\_folder:

  region:

\# Uncomment this to run plugin tests with standart redmine script

\# test:

\#   access\_key\_id:

\#   secret\_access\_key:

\#   bucket:

Now download dependency

- Bundle install 

root\@postgres-redmine:/usr/src/redmine# Bundle install

Then run this command 

- rake redmine:plugins

root\@postgres-redmine:/usr/src/redmine#rake redmine:plugins

![](https://lh7-us.googleusercontent.com/SghqxoFbw4IZ6DwFQk_DsB3McOcCCPE97_EDsRe9Amas79plzbsx8T8iZ3jMmWElWceHtSF0DpHlm_Vg1MJH8tPLfuyCx3LTwcZzaw_S5H6ZBx1qoftHaSWvef0hbcwPKsC_dVpoJXSZRaH4sriRBhQ)

Now you can see the plugins in redmine : 

Click on the administration option and click on the plugins options .

![](https://lh7-us.googleusercontent.com/AYeaRF75cLpoVqh-JPzcFwFeyjpUvmj3axoHsZTnnfZ9k9XStMutlYDzyLc9jujhfoIhP4Ve_eOBpfIVdzuM2FNvK4pByHIGzYk-vyOiBCcA0eWA4JcfD9EL8TqYAGR-n3dmCYeWWikLYzYdxvtnhJU)

**Restart your pod and server:-**

- Podman pod restart postgres-redmine
- Podman ps

![](https://lh7-us.googleusercontent.com/DGeFhIFOhoGfTgFIF1KGiTKgf2tj6CFosi3i-BUY7QUhTX1BdKDnoKea0xocOqA9wIA4u6EvdT8k8V3_TAGztPC_DhSFceGfvHGW5_oEEmPKlj_Y3LpOTB0qpPbtp9AoIFruGBVBb547sAkWTFt_0mg)

**Create image of container**

Commit command is used to create new container image

- podman commit redmine-app-new localhost/redmine-app-new-pandey-v1

![](https://lh7-us.googleusercontent.com/5pQS6gd4-IZ3K9bgoBxNdkkWHlqyfiaPcrzi09ZqVg6WXbD5s6t75ldoU_iSjW88j3G8u4DM54RxZCxqHobPhXMEBy2k7H-NnHPwXNUeYXmoMaIObAkxAzrLyI4H-BfV7THbNjPTehLX0_Zc8cJTJ_M)

**Search container image**

- Podman images

****![](https://lh7-us.googleusercontent.com/dh7qFLzSVdEe0w3dw-A5BVNUWiqiionXK3BW0yAG58d4mcaGVWEr69C2rfnm7tE00u5WQO_P88rTR4oDTjX6zLNRxoob9xU_3MEb2qfYQiKRYk1pqtSC8Mvi4Gr7YdOvfNp06nclNZUutcmq2Ir95ec)****

**6.Testing and test case :-**

**Creating Ticket:-** 

Login your own username and password.

![](https://lh7-us.googleusercontent.com/FwyTDoCBSNS3SUBHCkRUvrMzKbyGPFWqR39ZQ1MJmYZboXXmund7_RmC6OxZ9oqBseLjcoFNKbR7cy7BSjBnkoCzF4jBV3QQYNNEU-ro-8erMZAH0gJJTQ4f0AcowF-KwKM2gQrpc8uQKek9ABxLl0A)

Then click on project options and click on the right side “new project”

![](https://lh7-us.googleusercontent.com/AMYfjKsHAtOXcj2Jeeho1WRjZ0oRpog8a4YpAzgTLB_phUfeVkXQ3EoaZU5NaA00NYLIH9-kGKn1a4jvB0YGuCl8koRDJcnxxaCNYj_Tv8KvBL02zHBtpfoIiu58f9XmRk7J5t3rSFosk0x9xxr-IwI)

And create a project eg-testminio,

![](https://lh7-us.googleusercontent.com/e6iC5Z4UxFyNE07ho1TrnFiMoDj8QRCut6y1VZ0cPTocTgFiV89f-z7-kzYq0qzXRC4JBQ7hA1NFtiNgmz0dNmV4D8UGtHdIV6H_IYdFiqwLRLXM0Iuz7Jg0QWtwT0IvBs9WjiMeSY-xx6JWoi-zT24)

Then click on the administration and manage the redmine project and issue settings .

![](https://lh7-us.googleusercontent.com/El2KbNFJiqusyq8nvuYiVJDW3BK6udpQLTf3NR5OPUuQykQGl5f0kGgb5dnERyx0qJ7Ku4wT7MJwR_3bMtTJg1FJBFgK0cLIoJolPKenvGXN6bD16JV_d5L7Yh21RRzzxgmCVSEIpK95Nl4Q-1Orrow)

After Configure the redmine setting click on the project and go to issue options and click on new issue options .

![](https://lh7-us.googleusercontent.com/Ny-EsFf9jgQg1WZjlaYi43GZ-mMk8ZZu1zlAnTqGqK6ErIYFED3egeoJd78KkXgb1f33oa23UhcYhRbVf9GrYowXSrkl3ItUvQw7d4Xrp2zO3GLFMZLFmt3MWVgN-pWX59sdcT6P8In7h-5NwXtrvo8)

Create new project task 

![](https://lh7-us.googleusercontent.com/8GYxTXczWjIV2FQUzm1JPl1uhvnswY5Td7Czx6jho1zdm6eRDvKLWpwgg5JsC8sPB4si-A61EKUv9B9arwZDO04y9aTVT9mrAl2Om-Ml8cscQOnP39TlAooVQz1fhS3LcMRgxr5VuFcAQNH56iWJ8tc)

**Artefact #1**

Upload file while create project task ,

Now here you can see the upload file “abc.txt”

![](https://lh7-us.googleusercontent.com/xD7mNm3VvUDJnKpF80x9tIFp1Ss1qEkusYaatBcOPS-BqvfyNSSexcmX7Z04VmwTbduH6E0TF2kcs6KjzJxyhHunIGqN7l-XT6pSuYLOzy0lOuvfeD4pKjnnX2kWQJRbV7io-0WL0BTsuzqHdIGFx3w)

**Artefact #2**

Here you can see all data has been shifted to Object storage

![](https://lh7-us.googleusercontent.com/rn4FaiH7HojpXZWNJJlGgW1v1VsBOFCGSm37SG8fVHzMUolpDrSl6qobU4U93oVNvPQpdEyn0by3Vq2eoQRBXvMCC-c4thUY4wwNEkUmPglc73F8f_TQD7iLEjtqsICRm4OhExdxraoMKeEtKbOxEfw)

**Artefact #3**

Data Has been deleted from local folder now local folder is blank

- Podman exec -it postgres-redmine /bin/bash

![](https://lh7-us.googleusercontent.com/SuQxhWM67PkB1v5gFIPTDaKxakkJq5So1nhnZr1knwToldO7sFC4k08gFTWAVlQtjDtz5u4kgDW5mFxx41Gsrc6TyM1Y6fvWKOFpjqqhhUBhJE5r237eFCiUbY5x_xrL_6GhoR8LrceceHKqUGbEOFw)

\
\


-  Ls and cd files 

![](https://lh7-us.googleusercontent.com/k5D8W7yeILuc03QH4WFiw1HOZZi3CMcRRZdAezqGeCyCt-xutn2y-vtLFV8bxgoFQHj7uMTdVCIdj5DJtGNUFhUDZqTaWSrPfhMROOlu2gZbCDRMk9n4LwVRSOZU0zt3UZgAyhBs5bAbh9TcpqscxMg)

\
\
\
\


**Artefact #4**

Able to download data from same ticket

![](https://lh7-us.googleusercontent.com/WNj32pyehHRUUDORkXO1FNR4d5n5jP2prTXsu1Lc1y1wmg47wz1ZhPtrzigokTsoexd81_ovAjo17437bcl5jCNKdU1wbAQrjEGbxcUMm6q8qZlvtc4C5aKbeCXQyji25gtmIbkxxEsMEXIDr7Oxjf8)

**7.Generate Yaml File:-**

Run this command to generate a yaml file.

podman generate kube \<pod name> > \<File Name>.yaml

**8.Launch Pod using yaml:-**

vivek\@vivek-HP-EliteBook-840-G2:\~$ podman generate kube postgres-redmine > kube-postgres-redmine.yaml

\
\
\


**Now you can see here yaml file eg-   kube-postgres-redmine:-**

**vivek\@vivek-HP-EliteBook-840-G2:\~$** cat kube-postgres-redmine.yaml

\# Save the output of this file and use kubectl create -f to import

\# it into Kubernetes.

\#

\# Created with podman-3.4.2

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

\- postgres

image: docker.io/library/postgres:latest

name: redmine-postgres-intigration

env:

\- name: POSTGRES\_DB

   value: redminedb

\- name: POSTGRE\_USER

   name: redmineuser

\- name: POSTGRES\_PASSWORD

   value: redmine123

\- name: PGDATA

   value: /var/lib/postgresql/data/pgdata

ports:

\- containerPort: 3000

   hostPort: 3000

\- containerPort: 5432

   hostPort: 5432

resources: {}

securityContext:

   capabilities:

     drop:

     - CAP\_MKNOD

     - CAP\_NET\_RAW

     - CAP\_AUDIT\_WRITE

tty: true

volumeMounts:

\- mountPath: /var/lib/postgresql/data

   name: home-vivek-redmine-postgres-uat-host-0

  - args:

\- rails

\- server

\- -b

\- 0.0.0.0

image: docker.io/library/redmine:latest

name: redmine-app-new

env:

\- name: REDMINE\_DB\_PORT

   value: 5432

\- name: REDMINE\_DB\_USERNAME

   value: redmineuser

\- name: REDMINE\_DB\_PASSWORD

   value: redmine123

\- name: REDMINE\_DB\_DATABASE

   value: redminedb

\- name: AWS\_REGION

   value: us-east-1

\- name: AWS\_ACCESS\_KEY\_ID

   value: lpKM3E9qzJ9PIB7Bqey3

\- name: AWS\_SECRET\_ACCESS\_KEY

   value: 3Yp1SMjwZYHQXfW9eT6DoW2MgwCtReKCl1QClNwJ

resources: {}

securityContext:

   capabilities:

     drop:

     - CAP\_MKNOD

     - CAP\_NET\_RAW

     - CAP\_AUDIT\_WRITE

tty: true

volumeMounts:

\- mountPath: /usr/src/redmine/files

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

**vivek\@vivek-HP-EliteBook-840-G2**

**Remove pod** 

vivek\@vivek-HP-EliteBook-840-G2:\~$ podman pod rm postgres-redmine --force

3d03764227139c340b7dabb6cca395930fb9d998f939652ce122b6bb569327cc

**Now Create pod using yaml command:**

- podman play kube kube-podtgres-redmine.yaml

vivek\@vivek-HP-EliteBook-840-G2:\~$ podman play kube kube-postgres-redmine.yaml

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
