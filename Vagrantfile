# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT

sudo apt-get update
sudo apt-get install curl software-properties-common vim unzip wget git openjdk-7-jre -y

SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provision "shell", inline: $script

  config.vm.define "gocd_server" do |gocd_server|
    gocd_server.vm.box = "opscode_ubuntu-14.04_provisionerless"
    gocd_server.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"

    gocd_server.vm.network :private_network, ip: "192.168.33.40"
    gocd_server.vm.hostname = "gocd-server"

    gocd_server.vm.provision "shell", inline: "wget -P /tmp https://download.go.cd/gocd-deb/go-server-15.2.0-2248.deb"
    gocd_server.vm.provision "shell", inline: "sudo dpkg -i /tmp/go-server-15.2.0-2248.deb"

    gocd_server.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end
  end

  config.vm.define "gocd_agent" do |gocd_agent|
    gocd_agent.vm.box = "opscode_ubuntu-14.04_provisionerless"
    gocd_agent.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"

    gocd_agent.vm.network :private_network, ip: "192.168.33.41"
    gocd_agent.vm.hostname = "gocd-agent"

    gocd_agent.vm.provision "shell", inline: "wget -P /tmp https://download.go.cd/gocd-deb/go-agent-15.2.0-2248.deb"
    gocd_agent.vm.provision "shell", inline: "sudo dpkg -i /tmp/go-agent-15.2.0-2248.deb"
    gocd_agent.vm.provision "shell", inline: "sudo sed -i 's/127.0.0.1/192.168.33.40/' /etc/default/go-agent && sudo service go-agent restart"

    gocd_agent.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end
  end

end
