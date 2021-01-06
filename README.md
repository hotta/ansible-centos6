# Ansible playbook for CentOS6

## Prerequisite(Test Environment)

- Vagrant + VirtualBox VM running CentOS 6.10
- typical installation process could be like this:

```bash
mkdir XXXX
cd XXXXX
vi Vagrantfile (See below.)
vagrant up
```
And then log in to the VM as user "vagrant".

### Sample Vagrantfile:

```Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos6"
  config.vm.network "private_network", ip: "192.168.56.2"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.hostname = "example.local"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end
  config.vm.provision "shell", inline: <<-SHELL
		sudo dnf -y update
    sudo dnf -y install git epel-release
  SHELL
end
```

## Create environment which ansile can run

```bash
$ sudo yum install -y https://repo.ius.io/ius-release-el6.rpm
$ sudo yum install -y python36-pip
$ python3.6 -m venv venv
$ . ./venv/bin/activate
(venv) $ python3.6 -m pip install --upgrade pip
(venv) $ pip install ansible
(venv) ansible-galaxy collection install community.mysql community.general

```

## Build environment 

```bash
(venv) $ git clone https://github.com/hotta/ansible-centos6.git
(venv) $ cd ansible-centos6
(venv) $ ansible-playbook jobs/base.yml
(venv) $ sudo reboot
```


```bash
(venv) $ ansible-playbook jobs/base.yml
```
