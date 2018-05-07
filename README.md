# Packer Example - Ubuntu 16.04 minimal Vagrant Box

**Current Ubuntu Version Used**: 16.04.4

**Pre-built Vagrant Box**:

  - [`vagrant init geerlingguy/ubuntu1604`](https://vagrantcloud.com/geerlingguy/boxes/ubuntu1604)
  - See older versions: http://files.midwesternmac.com/

This example build configuration installs and configures Ubuntu 16.04 x86_64 minimal using Ansible, and then generates two Vagrant box files, for:

  - VirtualBox
  - VMware

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

### packer

- [Packer](http://www.packer.io/)


- [./docs/packer-install-to-users-bin.md](./docs/packer-install-to-users-bin.md)

### vagrant

  - [Vagrant](http://vagrantup.com/)

### virtualbox

  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)

### vmware

  - [VMware Fusion](http://www.vmware.com/products/fusion/) (or Workstation - if you want to build the VMware box)

### ansible

  - [Ansible](http://docs.ansible.com/intro_installation.html)

## Usage

### Disable NFS requirement

You will want to disable the NFS files sharing option if the system where you are running the VM's is not configured for NFS.

#### Edit Vagrantfile

Comment out the `  config.vm.synced_folder '.', '/vagrant', type: 'nfs'` line as follows:

```shell
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
#  config.vm.synced_folder '.', '/vagrant', type: 'nfs'
```

#### Edit packer-ubuntu-126.04/ansible/main.yml

Comment out the `    - geerlingguy.nfs` line

```shell
---
- hosts: all
  become: yes
  gather_facts: yes

  roles:
#    - geerlingguy.nfs
    - geerlingguy.packer-debian

  tasks:
    - apt: "name={{ item }} state=installed"
      with_items:
        - git
        - wget
        - curl
        - vim
```

### Create your box

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file. 

If you want to only build a box for one of the supported virtualization platforms (e.g. only build the VMware box), add `--only=vmware-iso` to the `packer build` command:

Build for virtualbox only

```shell
packer build --only=virtualbox-iso ubuntu1604.json
```

Build for vmware only

```shell
packer build --only=vmware-iso ubuntu1604.json
```

Build all (VMware and VirtulaBox) :

    packer build ubuntu1604.json

After a few minutes, Packer should tell you the box was generated successfully.


## Testing built boxes

There's an included Vagrantfile that allows quick testing of the built Vagrant boxes. From this same directory, run one of the following commands after building the boxes:

For VirtualBox:

```shell
vagrant up virtualbox --provider=virtualbox
```

For VMware Fusion:

    vagrant up vmware --provider=vmware_fusion
## Uploading to Vagrant

If you want your boxes available "everwhere" you can create a free account on **Vagrant Cloud. My**. The example I created is here:

```shell
https://app.vagrantup.com/csteel/boxes/Ubuntu1604
```



## License

MIT license.

## Author Information

Created in 2016 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).

## Changes Author

Wherever this differs from the original repository change have been made by:

2018 Christopher Steel