# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "parallels/ubuntu-14.04"

  config.vm.provider "parallels" do |v|
    v.name = "development"
    v.optimize_power_consumption = false
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.define :development do |t|
  end

  config.vm.synced_folder "~", "/vagrant"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y software-properties-common
    add-apt-repository -y ppa:ansible/ansible
    apt-get update
    apt-get -y dist-upgrade
    apt-get install -y ansible python-passlib python-netaddr python-augeas python-pip git
    pip install debops
    su vagrant -c debops-update
    apt-get install -y curl unzip
    curl -O -L https://dl.bintray.com/mitchellh/packer/packer_0.8.1_linux_amd64.zip && unzip packer_0.8.1_linux_amd64.zip -d /usr/bin/ && rm packer_0.8.1_linux_amd64.zip
    curl -O -L https://dl.bintray.com/mitchellh/terraform/terraform_0.6.0_linux_amd64.zip && unzip terraform_0.6.0_linux_amd64.zip -d /usr/bin/ && rm terraform_0.6.0_linux_amd64.zip
    su vagrant -c 'echo ". ~/work/environment" >> ~/.bashrc'
    ln -nfs /vagrant/.ssh/\* /home/vagrant/.ssh/
    ln -nfs /vagrant/work /home/vagrant/work
  SHELL
end
