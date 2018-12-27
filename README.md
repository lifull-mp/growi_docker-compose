# Vagrant - Docker and Docker Compose - Growi

Ref: 
  > https://github.com/weseek/growi-docker-compose
  >
  > https://github.com/weseek/growi
  >
  > https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7)
  >

## Install vagrant in windows:

* After download and install vagrant, run these commands below:

Based on this link, we can discovery vagrant boxes: https://app.vagrantup.com/boxes/search
>PS D:\> vagrant box add centos/7  # download centos 7 in your local
>
>PS D:\> vagrant box add ubuntu/xenial64  # download ubuntu in your local
>
>PS D:\> vagrant box update # update new version of boxes
>
>PS D:\> vagrant box list # list all boxes that existed in your local
>
>PS D:\> vagrant init centos/7

* Edit Vagrantfile with below information:

>  config.vm.box = "centos/7"
>
>  config.vm.network "public_network", bridge: "DW1520 Wireless-N WLAN Half-Mini Card"
>
>  config.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2223
>
>  config.vm.synced_folder "E:\docker\growi_docker-compose", "/home/vagrant/", nfs: true, owner: "vagrant", group: "vagrant"
>
>  config.vm.provider "virtualbox" do |vb|
>
>     vb.gui = false
>
> 	  vb.memory = "2048"
>
>  end

Note:
  public_network --> bridge the network of centos with your network card that will be easier to use from outsite
  
* Start vagrant

> vagrant up
>
> vagrant ssh
>

PS: we also can use ssh by adding public key (NAT port --> to access from outside)

## Install docker and docker-compose

Docker allow to create container from Dockerfile
Docker allow to create many containers from docker-compose.yml

### Step 1 — Install Docker

* Install needed packages:
> $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
  
* Configure the docker-ce repo (the repo may change --> need to check on docker's website)
> $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

* Install docker-ce:
> $ sudo yum install docker-ce
There are Docker EE (Enterprise Edition), Docker CE (Community Edition) versions.

* Add your user to the docker group with the following command.
> $ sudo usermod -aG docker $(whoami)

* Set Docker to start automatically at boot time:
> $ sudo systemctl enable docker.service

* Finally, start the Docker service:
> $ sudo systemctl start docker.service

 ### Step 2 — Install Docker Compose
 
 * Install Extra Packages for Enterprise Linux
> $ sudo yum install epel-release

* Install python-pip
> $ sudo yum install -y python-pip

* Then install Docker Compose:
> $ sudo pip install docker-compose

* You will also need to upgrade your Python packages on CentOS 7 to get docker-compose to run successfully:
> $ sudo yum upgrade python*

* To verify a successful Docker Compose installation, run:
> $ sudo docker-compose version

## Install growi by using docker-compose:

* To download sources
> $ git clone https://github.com/weseek/growi-docker-compose.git growi
>
> $ cd growi
>

* Edit configuration:
To allow access from others:
> services:
>
>  app:
>
>    ports:
>    
>      - 3000:3000
>

Enhance heap size for Elasticsearch if necessary
>environment:
>
>  - "ES_JAVA_OPTS=-Xms2g -Xmx2g"
>  

* Start growi
> $ sudo docker-compose up

* Checking:
```
[vagrant@centos7 growi]$ docker images -a
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
growi_app           latest              d15224166faa        3 hours ago         257MB
<none>              <none>              2a85b2fab9cd        3 hours ago         257MB
<none>              <none>              422a7a5a0964        3 hours ago         249MB
<none>              <none>              b1fad645638f        3 hours ago         249MB
weseek/growi        3                   68eba4a345e1        5 days ago          249MB
mongo               3.4                 9467ec7b04e5        5 weeks ago         361MB
docker/compose      1.23.1              6c16becf4900        7 weeks ago         63.5MB
hello-world         latest              4ab4c602aa5e        3 months ago        1.84kB
elasticsearch       5.3-alpine          c818119f17a4        20 months ago       123MB
[vagrant@centos7 growi]$ 
```

Docker-compose created 3 images: growi_app, mongo:3.4, elasticsearch:5.3-alpine
```
[vagrant@centos7 growi]$ docker ps -a
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS                   PORTS                    NAMES
fed087e8330e        growi_app                  "/docker-entrypoint.â€¦"   2 hours ago         Up 2 hours               0.0.0.0:3000->3000/tcp   growi_app_1
a8239aa3a022        mongo:3.4                  "docker-entrypoint.sâ€¦"   3 hours ago         Up 2 hours               27017/tcp                growi_mongo_1
26a7473542cf        elasticsearch:5.3-alpine   "/docker-entrypoint.â€¦"   3 hours ago         Up 2 hours               9200/tcp, 9300/tcp       growi_elasticsearch_1
969a9c2eb609        hello-world                "/hello"                 3 hours ago         Exited (0) 3 hours ago                            agitated_mendeleev
[vagrant@centos7 growi]$ 
```
