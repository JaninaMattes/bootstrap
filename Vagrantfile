# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |vb|
     vb.gui = true
  
     vb.memory = "1024"
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    
    sudo echo "LANG=en_US.UTF-8" >> /etc/environment
    sudo echo "LANGUAGE=en_US.UTF-8" >> /etc/environment
    sudo echo "LC_ALL=en_US.UTF-8" >> /etc/environment
    sudo echo "LC_CTYPE=en_US.UTF-8" >> /etc/environment
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get -y upgrade
    sudo apt-get -y install openjdk-11-jdk
    
    sudo apt-get install -y xfce4 virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11
    sudo apt-get install gnome-icon-theme-full tango-icon-theme
    sudo echo "allowed_users=anybody" > /etc/X11/Xwrapper.config

    sudo /usr/share/debconf/fix_db.pl
    sudo sudo apt-get -y upgrade

#    if ! [ -f /opt/eclipse-committers-oxygen-3-linux-gtk-x86_64.tar.gz ]; then
#        sudo wget -O /opt/eclipse-committers-oxygen-3-linux-gtk-x86_64.tar.gz http://ftp.fau.de/eclipse/technology/epp/downloads/release/oxygen/3/eclipse-committers-oxygen-3-linux-gtk-x86_64.tar.gz
#        cd /opt/ && sudo tar -zxvf eclipse-committers-oxygen-3-linux-gtk-x86_64.tar.gz
#    fi

#    curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
#    sudo apt-get install -y nodejs
#    echo "installed node js"

    sudo apt-get -q -y install git apache2-utils unzip

    sudo apt-get -q -y install software-properties-common

    sudo apt-get -q -y install ansible python-pip python-dev libffi-dev libssl-dev python-configparser

    sudo pip install pytz shade boto boto3
    echo "installed aws tooling and ansible"

    if ! [ -f /bluemixcli.installed ]; then
        sudo curl -sL https://ibm.biz/idt-installer | bash

        sudo touch /bluemixcli.installed
        echo "installed bluemix cli"
    fi

    export VER="0.11.12"
    if ! [ -f terraform_${VER}_linux_amd64.zip ]; then
        wget https://releases.hashicorp.com/terraform/${VER}/terraform_${VER}_linux_amd64.zip
        unzip terraform_${VER}_linux_amd64.zip
        sudo mv terraform /usr/local/bin/
    fi

    export TERRAFORM_VERSION="v0.15.1"
    wget https://github.com/IBM-Cloud/terraform-provider-ibm/releases/download/${TERRAFORM_VERSION}/linux_amd64.zip
    unzip linux_amd64.zip
    ln -f -s terraform-provider-ibm_${TERRAFORM_VERSION} terraform-provider-ibm
    mkdir -p .terraform.d/plugins
    mv terraform-provider-ibm* .terraform.d/plugins/




SHELL
  config.vm.synced_folder "workspace/", "/home/vagrant/workspace/", create: true
end
