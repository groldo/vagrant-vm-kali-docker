# vagrant-vm-kali-docker

This Vagrantfile installs a kali vm from
vagrant box kalilinux/running.
It also installs docker via ansible from
```https://github.com/groldo/docker-role.git```.

## Requirements

* vagrant
* ansible-galaxy and therefore ansible

If you prefer to not install ansible on your host,
you might can install all the requirements with git.

Some common shared files getting coppied to the guest system.
If you prefer them not to be shared, then just comment them out.
For vagrant to run, these files has to be there, since vagrant will stop with an error if they are not present.

```perl
config.vm.provision "file", source: "~/.gitconfig", destination: "$HOME/.gitconfig"
config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "$HOME/.ssh/authorized_keys"
config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "$HOME/.ssh/id_rsa.pub"
config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "$HOME/.ssh/id_rsa"
```

## Install

In the vars section of the playbook some variables are declared
You should change them to you're needs
```
    repos: [
      "https://github.com/hugsy/gef.git",
      "https://github.com/groldo/dotfiles.git"
    ]
    packages: [
      "texlive-latex-extra",
      "texlive-fonts-extra",
      "texlive-lang-german",
      "pandoc",
      "gdb",
      "vim",
      "tmux"
    ]
```

```bash
mkdir sync # the vagrantfile mounts a sync directory from pwd to the home directory
git clone https://github.com/groldo/vagrant-vm-kali-docker.git
ansible-galaxy install -r roles/requirements.yml -p roles/
vagrant up
```

First setup needs some time because the box has to be downloaded
and the packages from kali are getting updated.

Afterwards
```perl
# config.ssh.private_key_path = "~/.ssh/id_rsa"
```
has to be uncommented because the provisioning has copied a new ssh pub key to the guest.
Otherwise vagrant will ask for the password for user vagrant.


