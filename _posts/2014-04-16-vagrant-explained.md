---
layout: post
title: Vagrant and why it is useful in learning Puppet
---

Vagrant, according to the [documentation](http://docs.vagrantup.com/v2/why-vagrant/), provides a "disposable environment and consistent workflow for developing and testing infrastructure management scripts. You can quickly test things like shell scripts, Chef cookbooks, Puppet modules, and more using local virtualization such as VirtualBox or VMware. Then, with the same configuration, you can test these scripts on remote clouds such as AWS or RackSpace with the same workflow."

I've been trying to dedicate more time to working with Puppet and needed a better way to quickly test my manifests on a fresh system. The vCHS automation team has demonstrated using Vagant with VMware fusion, but I wanted to be able to use it in my homelab, natively with vSphere/ESXi. The vagrant-vsphere plugin provides the ability to use simple commands to deploy a vm/template, apply a specified manifest and even SSH to the freshly create vm. 

This will be the first of several blog posts in which I will cover:

* Installing Vagant and the vagrant-vsphere plugin on a CentOS 6.x virtual machine
* Creating a CentOS 6.x template that is customized for Vagrant
* Defining a Puppet manifest that will automatically run when a template is deployed


These posts will hopefully help you understand the steps necessary to get vagrant installed and working with a vSphere environment.
