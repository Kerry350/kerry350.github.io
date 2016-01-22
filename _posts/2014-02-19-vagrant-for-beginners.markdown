---
title: Vagrant for beginners
date: 2014-02-19 21:56:47 
tags: 
layout: post
---
Vagrant has been around for ages now. I've always loved the premise - i.e. being able to "Create and configure lightweight, reproducible, and portable development environments.". I loved the idea. For a while though I was reading a lot of bad reports, not about Vagrant as a whole or even what it was trying to achieve, but just those niggly faults like "Shit! I can't SSH in to Vagrant". Those sorts of things put me off, because there's nothing worse than not being able to use your development environment proficiently. But those sorts of complaints calmed down and Vagrant matured a lot, so I decided to give it another go recently. Good news, it's awesome. 

So I thought I'd give a little run down to people who haven't used Vagrant before on how to get started. Some of the tutorials out there are quite heavy and cause more confusion than understanding. 

1) Download and install [VirtualBox ](https://www.virtualbox.org/wiki/Downloads)

2) Download and install [Vagrant](http://www.vagrantup.com/downloads.html)

3) `cd` in to the directory of the project that you wish to use Vagrant with, run `vagrant init` to create a Vagrantfile for the project. 

4) Now we need to add a 'box' to Vagrant. This is considered your 'base', i.e. Ubuntu, CentOS, Debian and so on. This will be added to Vagrant itself, and not specific to your project, so you can use this box as many times as you want. Run `vagrant box add precise32 http://files.vagrantup.com/precise32.box` to add a Ubuntu box. 

5) We'll now edit our Vagrantfile that was added for us by running `vagrant init`. Add `config.vm.box = "precise32"` to tell Vagrant to use our new Ubuntu box. 

6) To boot the guest machine (via Virtualbox), using the box we configured, run `vagrant up`. Boom! We now have a fully-functional virtual machine running the box we requested, in this case Ubuntu.

7) Vagrant runs the VMs without a UI, to interact with the VM we can use `vagrant ssh`. You can do anything in here as you normally would on your own machine. Install stuff, move directories etc. 

8) If we wanted to kill this Vagrant instance and get rid of all traces of it we just need to run `vagrant destroy`. 

9) By default, Vagrant shares your project directory (the one with the Vagrantfile) to the `/vagrant` directory in your VM. This is called 'synced folders' and means we can continue editing things in our project directory as we normally would, whilst having the files sync through to the VM, or vice versa. 

10) So now we really need to think about reproducible environments. Vagrant has provisioning built in by way of Shell script, Puppet, Chef, Docker, Ansible or Salt. When we `vagrant up` any necessary provisioning steps will be run for us; software will be installed and configured. Ideally Puppet or Chef would be used for this, but for now we'll use a Shell script to get us started. 

11) Make a file called `bootstrap.sh` in your project directory. 

12) For now we'll install some core packages and then install Node.js. Pop this in your `bootstrap.sh` file. The `-y` flag is important, it automates prompts being answered with a yes. Don't sit there wondering why installs weren't being performed like me... 

```
#!/usr/bin/env bash

apt-get update -y

apt-get install make -y
apt-get install build-essential -y
apt-get install openssl -y
apt-get install libssl-dev -y 
apt-get install pkg-config -y

wget http://nodejs.org/dist/v0.10.25/node-v0.10.25.tar.gz

tar -zxf node-v0.10.25.tar.gz 
cd node-v0.10.25
./configure
make
sudo make install
```
13) Update the Vagrantfile to use this bootstrap shell script to provision with. Add `config.vm.provision :shell, :path => "bootstrap.sh"` to the Vagrantfile. 

14) Now, whilst `vagrant up` would run our provisioning for us at this point, we're already running the VM, so instead we'll just use `vagrant reload --provision`, which will skip the boot phase and just provision for us. 

15) Now we'll enable some networking features, this is just so we can access web servers etc that are running on our VM via our Host machine's web browser. We'll use port forwarding as that's the easiest method to get started with. Add `config.vm.network :forwarded_port, host: 4567, guest: 3000` to our Vagrantfile. Run `vagrant up` or `vagrant reload` depending on whether your VM is running. Now if we were to run, for example, an Express instance listening on port 3000 from the VM we'd be able to access this at `localhost:4567` on our own machine. Sweet. 

16) When we're done with our project - either for an extended period of time or until work the next day we can make use of [`suspend`, `halt` or `destroy`](http://docs.vagrantup.com/v2/getting-started/teardown.html), which all have slightly different characteristics. 

That's all the basics of Vagrant. You'd possibly want to look in to more advanced provisioning with something like Puppet. But commit your Vagrantfile to source control and you have a team of developers all `vagrant up`-ing with the exact same environment. If you have 4 projects all using different versions of Node.js, no problem! When you're done with a VM, or you screw up configuring it (we've all been there), bail out with a `vagrant destroy` - no one will ever know. It can be your dirty little secret. 
