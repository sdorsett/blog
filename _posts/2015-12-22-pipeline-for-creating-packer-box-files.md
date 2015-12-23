---
layout: post
title: Setting up a pipeline for creating packer .box files  
---

Recently at work, the vCloud Air Zombie team has been using packer to generate Vagrant templates for use in development and testing.  I have previously covered [how to use packer to create create a .box template for use with Vagrant](https://sdorsett.github.io/2015/01/03/using-packer-on-centos/), but I thought it might be useful to others to have me demonstrate how we are using Packer within our development team.

This will be the first of several blog posts in which I intend to cover:

* [Installing a ESXi 6.0 virtual machine for use with Packer](https://sdorsett.github.io/2015/12/23/installing-esxi-virtual-machine-for-packer-depolyment/).
* Setting up Packer, ovftool and Apache web server on a CentOS virtual machine.
* Creating up our first packer template for installing CentOS 6.7 and vmtools.
* Installing the Puppet agent in our CentOS 6.7 template .
* Using ovftool to convert Packer generated virtual machines, into Vagrant .box files
* Hosting the resulting Vagrant .box files on a web server so they can be downloaded by Vagrant.

These posts will hopefully be helpful by providing details on the process of using Packer to generate Vagrant .box files.
