# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "kalilinux/rolling"
  config.vm.synced_folder "sync/", "/home/vagrant/sync"
  config.vm.define "foo"
  config.vm.hostname = "bar"
  config.vm.provider :virtualbox do |vb|
    vb.name = "foobar"
    vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
    vb.customize ["modifyvm", :id, "--vrde", "off"]
    vb.customize ["modifyvm", :id, "--memory", "4096"]
    vb.customize ["modifyvm", :id, "--audioout", "on"]
    vb.gui = false
  end

  # activate after provisioning
  # config.ssh.private_key_path = "~/.ssh/id_rsa"
  config.ssh.username = "vagrant"
  config.ssh.insert_key = "true"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.install = "true"
    ansible.install_mode = "default"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    ansible.galaxy_role_file = "roles/requirements.yml"
  end

  config.vm.provision "file", source: "~/.gitconfig", destination: "$HOME/.gitconfig"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "$HOME/.ssh/authorized_keys"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "$HOME/.ssh/id_rsa.pub"
  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "$HOME/.ssh/id_rsa"
end
