# Vagrant - Docker and Docker Compose - Growi

Ref: 
  https://github.com/weseek/growi-docker-compose 
  https://github.com/weseek/growi

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
> $ docker-compose version
