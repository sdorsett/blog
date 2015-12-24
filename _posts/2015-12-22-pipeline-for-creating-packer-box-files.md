---
layout: post
title: Setting up a pipeline for creating Packer .box files  
---

Recently at work, the vCloud Air Zombie team has been using Packer to generate Vagrant templates for use in development and testing.  I have previously covered [how to use Packer to create create a .box template for use with Vagrant](https://sdorsett.github.io/2015/01/03/using-packer-on-centos/), but I thought it might be useful to others to demonstrate how we are using Packer to create images.

This will be the first of several blog posts in which I intend to cover:

* [Installing a ESXi 6.0 virtual machine for use with Packer](https://sdorsett.github.io/2015/12/23/installing-esxi-virtual-machine-for-packer-depolyment/) - this is what Packer will be using to create the image.
* [Setting up Packer, ovftool and Apache web server on a CentOS virtual machine](https://sdorsett.github.io/2015/12/24/installing-packer-and-ovftool-on-centos/) - this will be where the Packer templates are run from.
* [Creating our first Packer template for installing CentOS 6.7 with vmtools](https://sdorsett.github.io/2015/12/25/creating-a-packer-template-for-installing-centos-67/) - templates are the "recipes" for what ingredients go into a Packer image.
* Copying our existing CentOS 6.7 template and adding the Puppet agent - having the puppet agent in an image allows us to use puppet to describe configurations using puppet in either Packer and Vagrant.
* Using ovftool to convert Packer generated virtual machines, into Vagrant .box files - ovftool allows you to export the Packer created images in either a Fusion or vSphere compatible format.
* Hosting the resulting Vagrant .box files on a web server - Hosting the .box files on a web server allows them to be easily used within Vagrantfiles.

These posts will hopefully be helpful by providing details on the process of using Packer to generate Vagrant .box files.
