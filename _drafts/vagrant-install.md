---
layout: post
title: Installing Vagrant & the vagrant-vsphere plugin
---

Vagrant, according to the [Vagrant documentation](http://docs.vagrantup.com/v2/why-vagrant/), "gives you a disposable environment and consistent workflow for developing and testing infrastructure management scripts. You can quickly test things like shell scripts, Chef cookbooks, Puppet modules, and more using local virtualization such as VirtualBox or VMware. Then, with the same configuration, you can test these scripts on remote clouds such as AWS or RackSpace with the same workflow."

Rather than using VirtualBox or VMware Fusion, the vagrant-vsphere plugin allows you to use VMware vCenter as the virtualization "backend" of vagrant. The following post will help you get Vagrant installed on a CentOS 6.x vm.


1. Create a CentOS 6.x minimal virtual machine for installing Vagrant. I already had an existing Centos 6.5 minimal template that I simply cloned for this purpose:

2. Power on the cloned vm and connect to the console:


3. Modify /etc/sysconfig/network, /etc/sysconfig/network-scripts/ifcfg-eth0 & /etc/resolv.conf to reflect the network settings for your vagrant vm:

[root@vagrant ~]# cat /etc/sysconfig/network 
NETWORKING=yes 
HOSTNAME=vagrant.mylab.net 
GATEWAY=192.168.1.1 
[root@vagrant ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0 
DEVICE=eth0 
TYPE=Ethernet 
ONBOOT=yes 
NM_CONTROLLED=no 
BOOTPROTO=static 
IPADDR=192.168.1.40 
NETMASK=255.255.255.0 
[root@vagrant ~]# cat /etc/resolv.conf
 search mylab.net 
nameserver 192.168.1.1 
nameserver 8.8.8.8 
nameserver 8.8.4.4

4. Restart the vm for the hostname change to take affect
5. SSH to the vagrant vm using the IP address you assigned in /etc/sysconf/network-scripts/ifcfg-eth0
6. Install Ruby 1.9.3. There are several ways to install ruby 1.9.3 on CentOS, but I'll cover using ruby-build to accomplish this:

  A. Use yum in install the packages we'll need to get ruby and vagrant installed:

yum install -y gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel iconv-devel libxml2 libxml2-devel libxslt libxslt-devel wget git

  B. Pull down the latest version of ruby-build using git:



Lanyon is an unassuming [Jekyll](http://jekyllrb.com) theme that places content first by tucking away navigation in a hidden drawer. It's based on [Poole](http://getpoole.com), the Jekyll butler.

### Built on Poole

Poole is the Jekyll Butler, serving as an upstanding and effective foundation for Jekyll themes by [@mdo](https://twitter.com/mdo). Poole, and every theme built on it (like Lanyon here) includes the following:

* Complete Jekyll setup included (layouts, config, [404](/404), [RSS feed](/atom.xml), posts, and [example page](/about))
* Mobile friendly design and development
* Easily scalable text and component sizing with `rem` units in the CSS
* Support for a wide gamut of HTML elements
* Related posts (time-based, because Jekyll) below each post
* Syntax highlighting, courtesy Pygments (the Python-based code snippet highlighter)

### Lanyon features

In addition to the features of Poole, Lanyon adds the following:

* Toggleable sliding sidebar (built with only CSS) via **☰** link in top corner
* Sidebar includes support for textual modules and a dynamically generated navigation with active link support
* Two orientations for content and sidebar, default (left sidebar) and [reverse](https://github.com/poole/lanyon#reverse-layout) (right sidebar), available via `<body>` classes
* [Eight optional color schemes](https://github.com/poole/lanyon#themes), available via `<body>` classes

[Head to the readme](https://github.com/poole/lanyon#readme) to learn more.

### Browser support

Lanyon is by preference a forward-thinking project. In addition to the latest versions of Chrome, Safari (mobile and desktop), and Firefox, it is only compatible with Internet Explorer 9 and above.

### Download

Lanyon is developed on and hosted with GitHub. Head to the <a href="https://github.com/poole/lanyon">GitHub repository</a> for downloads, bug reports, and features requests.

Thanks!
