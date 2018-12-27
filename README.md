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
> 	vb.gui = false
>
> 	  vb.memory = "2048"
>
>  end

