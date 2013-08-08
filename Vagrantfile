# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network :private_network, ip: "172.16.0.1"
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 5000, host: 5000
  config.vm.network :forwarded_port, guest: 9001, host: 9001

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  
  if Dir["../designate"] != nil
    config.vm.synced_folder "../designate", "/opt/stack/designate"
  end

  if Dir["../python-designateclient"] != nil
    config.vm.synced_folder "../python-designateclient", "/opt/stack/python-designateclient"
  end

  if Dir["../horizon"] != nil
    config.vm.synced_folder "../horizon", "/opt/stack/horizon"
  end
  
  config.vm.provision :shell do |shell|
    shell.inline = %Q{
      if [ ! -f "/usr/bin/git" ]; then
        apt-get update
        apt-get install --yes git
      fi

      if type -p screen >/dev/null && sudo -i -u vagrant screen -ls | egrep -q "[0-9].stack"; then
        sudo -i -u vagrant /vagrant/unstack.sh
      fi

      sudo -i -u vagrant /vagrant/stack.sh
    }
  end

end
