# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "parallels/ubuntu-14.04"

  config.vm.provider "parallels" do |v|
    v.name = "development"
    v.optimize_power_consumption = false
    v.memory = 2048
    v.cpus = 1
  end

  config.vm.define :development do |t|
  end

  ports = (3000..3010).to_a + [1080, 8080, 9200, 9292]
  ports.each do |port|
    config.vm.network "forwarded_port", guest: port, host: port
  end

  config.vm.synced_folder "~", "/vagrant"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y software-properties-common
    add-apt-repository -y ppa:ansible/ansible
    apt-get update
    apt-get -y dist-upgrade
    apt-get install -y ansible python-passlib python-netaddr python-augeas python-pip git vim
    apt-get install -y curl unzip
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ln -nfs /vagrant/.ssh/\* ~/.ssh/
    ln -nfs /vagrant/work ~/work
    ln -nfs /vagrant/key ~/key

    curl -sS -O -L https://dl.bintray.com/mitchellh/packer/packer_0.8.1_linux_amd64.zip
    mkdir -p ~/key/third_party/linux/packer
    unzip -o packer_0.8.1_linux_amd64.zip -d ~/key/third_party/linux/packer/
    rm packer_0.8.1_linux_amd64.zip

    curl -sS -O -L https://dl.bintray.com/mitchellh/terraform/terraform_0.6.0_linux_amd64.zip
    mkdir -p ~/key/third_party/linux/terraform
    unzip -o terraform_0.6.0_linux_amd64.zip -d ~/key/third_party/linux/terraform/
    rm terraform_0.6.0_linux_amd64.zip

    echo '. ~/work/environment' >> ~/.bashrc
    cd ~/work/ansible && ansible-playbook -i 'localhost,' -u vagrant -s -c local development.yml
  SHELL
end
