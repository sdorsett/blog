---
layout: post
title: Using Vagrant to test our CentOS 6.7 Packer template
---

In the last posts we covered [installing a ESXi virtual machine for use with Packer](https://sdorsett.github.io/2015/12/23/installing-esxi-virtual-machine-for-packer-depolyment/) and [Setting up Packer, ovftool and Apache web server on a CentOS virtual machine](https://sdorsett.github.io/2015/12/24/installing-packer-and-ovftool-on-centos/). In this post we will be putting all this prep work to use in order to install a CentOS 6.7 image using Packer.

Before we get started I would like to mentioned that many of the configuration files I am using have been influenced by or directly copied from the following github repositories:  

[https://github.com/frapposelli/packer-templates](https://github.com/frapposelli/packer-templates/)  
[https://github.com/nanliu/packer-templates/](https://github.com/nanliu/packer-templates/)  

These public repositories by Fabio and Nan have been extremely helpful in my own understanding of how Packer works.
 Well...enough talking, let's get started...

## 1. Download the Vagrant 1.7.4 RPM and install it. I've had issues with making the vagrant-vcenter NFS folder sync work with Vagrant 1.8.0 and newer.

{% highlight bash %}
[root@packer-centos ~]# rm vagrant_1.7.4_x86_64.rpm
rm: remove regular file `vagrant_1.7.4_x86_64.rpm'? y
[root@packer-centos ~]# wget https://releases.hashicorp.com/vagrant/1.7.4/vagrant_1.7.4_x86_64.rpm
--2015-12-25 16:44:15--  https://releases.hashicorp.com/vagrant/1.7.4/vagrant_1.7.4_x86_64.rpm
Resolving releases.hashicorp.com... 23.235.44.69
Connecting to releases.hashicorp.com|23.235.44.69|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 72907475 (70M) [binary/octet-stream]
Saving to: “vagrant_1.7.4_x86_64.rpm”

100%[========================================================>] 72,907,475  6.96M/s   in 10s

2015-12-25 16:44:26 (6.94 MB/s) - “vagrant_1.7.4_x86_64.rpm” saved [72907475/72907475]

[root@packer-centos ~]#
[root@packer-centos ~]# rpm -i vagrant_1.7.4_x86_64.rpm
[root@packer-centos ~]# vagrant -v
Vagrant 1.7.4
[root@packer-centos ~]#
{% endhighlight %}

## 2. Install the vagrant-vcenter provider

{% highlight bash %}
[root@packer-centos ~]# vagrant plugin install vagrant-vcenter
Installing the 'vagrant-vcenter' plugin. This can take a few minutes...
Installed the plugin 'vagrant-vcenter (0.3.2)'!
[root@packer-centos ~]# vagrant plugin list
vagrant-share (1.1.4, system)
vagrant-vcenter (0.3.2)
[root@packer-centos ~]#
{% endhighlight %}

## 3. Create directory structure for the .iso files, scripts and templates we will be using with Packer.

{% highlight bash %}
[root@packer-centos packer-templates]# mkdir -p ~/packer-templates/{iso,scripts,templates}
[root@packer-centos packer-templates]# ls
iso  scripts  templates
[root@packer-centos packer-templates]#
{% endhighlight %}

###This seems like a good point to take break. We covered a lot of ground in this post, but as a result of all our work we have a working Packer template to build on.  

###If you would like to not have to manually create the files covered in this post, you can clone down [this github repository](https://github.com/sdorsett/packer-templates/tree/first-packer-template) by running the following command:

<pre>
git clone -b "first-packer-template" https://github.com/sdorsett/packer-templates.git
</pre>  

### You will still need to create the iso/ directory, download the needed .iso files and create the packer-remote-info.json file as covered in this post.  

###In the next post we will extend what was covered in this post by installing the Puppet agent in Packer image.

###Please provide any feedback or suggestions to my twitter account located on the about page.
