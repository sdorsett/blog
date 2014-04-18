---
layout: post
title: Installing Vagrant and the vagrant-vsphere plugin on CentOS 6.x
---

Installing Vargrant and the vagrant-vsphere plugin on CentOS 6.x

Some readers might find this post to be a little "in the weeds" and cover details they are already familiar with. I struggled in my first attempting to get Vagrant working on CentOS, becuase I couldn't find any good tutorials that covered the entire process. If you get bored with basic network configuration or have a more preferred method for installing Ruby, please understand I'm trying to provide as much detail as possible to those without much linux experience. I also also assuming you have a working vCenter, ESXi hosts and dhcp server configured to assign IP addresses for the Vagrant virtual machines we will be deploying.

1. Create a CentOS 6.x minimal virtual machine for installing Vagrant. 
Create or clone a fresh CentOS 6.x minimal virtual machine. I already had an existing Centos 6.5 minimal template that I simply cloned for this purpose.



2. Power on the new cloned vm and connect to the virtual machine console

![My helpful screenshot](/assets/01-clone-existing-template.png)

3. Modify /etc/sysconfig/network, /etc/sysconfig/network-scripts/ifcfg-eth0 & /etc/resolv.conf to reflect the network settings for your vagrant vm:

{% highlight bash %}
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
{% endhighlight %}

4. Restart the vm for the hostname change to take affect
5. SSH to the vagrant vm using the IP address you assigned in /etc/sysconf/network-scripts/ifcfg-eth0
6. Install Ruby 1.9.3. There are several ways to install ruby 1.9.3 on CentOS, but I'll cover using ruby-build to accomplish this:

  A. Use yum in install the packages we'll need to get ruby and vagrant installed:

yum install -y gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel iconv-devel libxml2 libxml2-devel libxslt libxslt-devel wget git

  B. Pull down the latest version of ruby-build using git:


which is shown in the screenshot below:
![My helpful screenshot]({{ site.url }}/assets/screenshot.jpg)


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
