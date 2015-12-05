##Description

This a install guide for setup development environment using [Vagrant](http://vagrantup.com/) and [Puppet](http://puppetlabs.com). There are two ways to manage the development:

1. maintain the vagrant configure-file and puppet configure-file. It is easy to add to repo using git, but it will spend more time to download and setup.
2. maintain the vagrant boxes. It is easy to deliver and share, but hard to add to repo because of its relatively large size.

So in there we just talk about the first way.


##Requirments

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant 1.1+](http://vagrantup.com/) (not a Ruby gem)

Vagrant base box running:

* [Ubuntu 12.04 - Precise Pangolin 64-bit](http://files.vagrantup.com/precise64.box)

##How To Build The Virtual Machine

Building the virtual machine is this easy:

	host $ mkdir -p hotspot-workspace/vagrant-share (explained blow)
	host $ cd hotspot-workspace 
    host $ git clone --recursive https://github.com/cloud-hot/hotspot-dev-environment.git (including all submodules )
    host $ cd hotspot-dev-environment
    host $ vagrant up

That's it.

If the base box is not present that command fetches it first. The setup itself takes about 3 minutes in my MacBook pro. After the installation has finished, you can access the virtual machine with

    host $ vagrant ssh
    Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic-pae i686)
    ...
    vagrant@hotspot-dev-box:~$

Port 6000 in the host computer is forwarded to port 6000 in the virtual machine. Thus, applications running in the virtual machine can be accessed via localhost:6000 in the host computer. By default the virtual machine can be accessed by host through ip: 192.168.2.2, if you want to modify the ip address or through public network you can modify the line in Vagrantfile:

     config.vm.network :private_network, ip: "192.168.2.2"

Shared folders between on the host machine to the guest machine is set to `../vagrant-share`. This path is relative to the project root `hotspot-dev-box`. You can modify the line in Vagrantfile according to yourself:

     config.vm.synced_folder "../vagrant-share", "/vagrant_data", type: "nfs"

All configuration related FAQ can be found in [Vagrant documentation](http://docs.vagrantup.com/v2/)

##What's In The Box

####DONE:

* [apache](https://github.com/puppetlabs/puppetlabs-apache.git)
* [apt](https://github.com/puppetlabs/puppetlabs-apt.git)
* [common](https://labs.riseup.net/code/projects/module-lsb)
* [git](https://github.com/puppetlabs/puppetlabs-git.git)
* [lsb](https://github.com/example42/puppet-lsb.git)
* [openssh](https://github.com/example42/puppet-openssh.git)
* [release](https://github.com/puppetlabs/puppetlabs-release.git)
* [rsync](https://github.com/puppetlabs/puppetlabs-rsync.git)
* [rvm](https://github.com/cloud-hot/puppet-rvm)
* [sqlite](https://github.com/puppetlabs/puppetlabs-sqlite.git)
* [stdlib](https://github.com/puppetlabs/puppetlabs-stdlib.git)
* [tftp](https://github.com/puppetlabs/puppetlabs-tftp.git)
* [vcsrepo](https://github.com/puppetlabs/puppetlabs-vcsrepo.git)
* [xinetd](https://github.com/puppetlabs/puppetlabs-xinetd.git)
* hotspot (own)

####TODO:

* Nginx

##Recommended Workflow

The recommended workflow is

* edit in the host computer and

* test within the virtual machine.

Just clone your applicantion fork into the `vagrant-share` directory on the host computer:

	host $ cd ../vagrant-share
    host $ git clone git@github.com:<your username>/app.git
	host $ ls
    app

Vagrant mounts that directory as `/vagrant_data` within the virtual machine:

    vagrant@hotspot-dev-box:~$ ls /vagrant_data
    app

We are ready to go to edit in the host, and test in the virtual machine.

This workflow is convenient because in the host computer you normally have your editor of choice fine-tuned, Git configured, and SSH keys in place.

##Virtual Machine Management

When done just log out with `^D` and suspend the virtual machine

    host $ vagrant suspend

then, resume to hack again

    host $ vagrant resume

Run

    host $ vagrant halt

to shutdown the virtual machine, and

    host $ vagrant up

to boot it again.

You can find out the state of a virtual machine anytime by invoking

    host $ vagrant status

Finally, to completely wipe the virtual machine from the disk **destroying all its contents**:

    host $ vagrant destroy # DANGER: all is gone

Please check the [Vagrant documentation](http://docs.vagrantup.com/v2/) for more information on Vagrant.