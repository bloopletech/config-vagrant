# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "parallels/ubuntu-14.04"

  config.vm.provider "parallels" do |v|
    v.name = "development"
    v.optimize_power_consumption = false
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.define :development do |t|
  end

  ports = (3000..3010).to_a + [1080, 1723, 4440, 5000, 8080, 9200, 9292, 35729]
  ports.each do |port|
    config.vm.network "forwarded_port", guest: port, host: port
  end

  config.vm.synced_folder "~", "/vagrant"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get -y dist-upgrade
    apt-get install -y software-properties-common git vim python-pip curl unzip
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ln -nfs /vagrant/.ssh/\* ~/.ssh/
    ln -nfs /vagrant/work ~/work
    ln -nfs /vagrant/key ~/key

#    curl -sS -O -L http://stedolan.github.io/jq/download/linux64/jq
#    chmod 755 jq
#    mv jq ~/key/third_party/linux/
  SHELL
end
