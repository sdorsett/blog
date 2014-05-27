---
layout: post
title: Installing Vagrant and the vagrant-vcloud plugin on CentOS 6.x
---

In this post I'm setting out to explain how to create a CentOS 6.4 vm, from template, in vCHS (or a vCloud Director instance) and then install vagrant-vcloud on that. 

### 1. Create a CentOS 6.x minimal virtual machine from a template in a vCHS organization.
 
I will demonstrate creating a new CentOS vm in vCHS using a template, but you could just as easily create a new CentOS virtual machine from scratch in vCloud Director.

#### A. Log into vCHS using your credentials and click on your virtual datacenter. 

#### B. Next click on the "Virtual Machines" tab and then click the "Deploy a Virtual Machine" button:

![screenshot]({{https://sdorsett.github.io }}/assets/01-create-centos-64-vm.png)

#### C. Select the "CentOS 6.4 64 Bit" template and click "Continue" 

![screenshot]({{https://sdorsett.github.io }}/assets/02-select-centos-64-template.png)

#### D. Name the virtual machine and set the guest OS name of your new virtual machine. Click "Deploy This Virtual Machine": 

I named my virtual machine and set the guest OS name both to "vagrant", but you can change these to what ever you prefer.

![screenshot]({{https://sdorsett.github.io }}/assets/03-virtual-machine-resource-settings.png)

#### E. Power on and then click the name of the virtual machine you just created: 

![screenshot]({{https://sdorsett.github.io }}/assets/04-centos-vm-created.png)

#### F. Click the "Launch Console" link to open the console of the virtual machine you just created. 

Notice that the guest OS password created by guest customization is displayed on the left. Log into the virtual machine, using "root" and the displayed password, then change this password immediately to something else:

![screenshot]({{https://sdorsett.github.io }}/assets/05-open-console-and-log-in.png)

### 2. Modify /etc/resolv.conf

{% highlight bash %}
[root@vagrant ~]# cat /etc/resolv.conf
nameserver 8.8.8.8 
nameserver 8.8.4.4
{% endhighlight %}

### 3. Install Ruby 1.9.3 using ruby-build:

#### A. Use yum to install the packages we'll need to get ruby, rubygems and vagrant installed:

{% highlight bash %}
[root@vagrant ~]# yum install -y gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel iconv-devel libxml2 libxml2-devel libxslt libxslt-devel wget git
{% endhighlight %}

#### B. Pull down the latest version of ruby-build using git:

{% highlight bash %}
[root@vagrant ~]# git clone https://github.com/sstephenson/ruby-build.git
{% endhighlight %}

#### C. Change to the directory the previous command created:

{% highlight bash %}
[root@vagrant ~]# cd ruby-build/
{% endhighlight %}

#### D. Run the 'install.sh' script to install ruby-build:

{% highlight bash %}
[root@vagrant ruby-build]# ./install.sh
{% endhighlight %}

#### E. I would recommend installing Ruby version 1.9.3-p545 with the following command:

{% highlight bash %}
[root@vagrant ruby-build]# ruby-build --definitionsruby-build 1.9.3-p545 /usr/local/
Downloading yaml-0.1.6.tar.gz...
-> http://dqw8nmjcqpjn7.cloudfront.net/5fe00cda18ca5daeb43762b80c38e06e
Installing yaml-0.1.6...
Installed yaml-0.1.6 to /usr/local/

Downloading ruby-1.9.3-p545.tar.gz...
-> http://dqw8nmjcqpjn7.cloudfront.net/8e8f6e4d7d0bb54e0edf8d9c4120f40c
Installing ruby-1.9.3-p545...

Installed ruby-1.9.3-p545 to /usr/local/
{% endhighlight %}

### 4. Install rubygems by downloading the latest version from [rubygems.org](https://rubygems.org/pages/download) and install it:

#### A. Use wget to download the latest version listed on the rubygems download page:

{% highlight bash %}
[root@vagrant ruby-build]# cd ~/
[root@vagrant ~]# wget http://production.cf.rubygems.org/rubygems/rubygems-2.2.2.tgz
{% endhighlight %}

#### B. Use tar to unzip the tar-gzipped file we downloaded:

{% highlight bash %}
[root@vagrant ~]# tar xvzf rubygems-2.2.2.tgz
[root@vagrant ~]# cd rubygems-2.2.2
{% endhighlight %}

#### C. Install rubygems by running the 'setup.rb' script:

{% highlight bash %}
[root@vagrant rubygems-2.2.2]# ruby setup.rb
{% endhighlight %}

### 5. Install Vagrant

#### A. Use wget to download the latest version listed on the [Vagrant download](https://www.vagrantup.com/downloads.html) page:

{% highlight bash %}
[root@vagrant rubygems-2.2.2]# cd ~/
[root@vagrant ~]# wget https://dl.bintray.com/mitchellh/vagrant/vagrant_1.6.2_x86_64.rpm
{% endhighlight %}

#### B. Use the 'rpm' command to install the RPM package we downloaded in the previous step:

{% highlight bash %}
[root@vagrant ~]# rpm -i vagrant_1.6.2_x86_64.rpm
{% endhighlight %}

#### C. Export the directory that the vagrant executable was install to, in order to make it easier to run:

{% highlight bash %}
[root@vagrant ~]# export PATH=$PATH:/opt/vagrant/bin/
{% endhighlight %}

### 6. Installing the vagrant-vcloud plugin

{% highlight bash %}
[root@vagrant blog]# vagrant plugin install vagrant-vcloud
Installing the 'vagrant-vcloud' plugin. This can take a few minutes...
Installed the plugin 'vagrant-vcloud (0.3.3)'!
{% endhighlight %}

### 7. Creating a Vagrantfile for testing vagrant-vcloud install 

#### A.  We need to create a directory structure and Vagrantfile to test that the vagrant-vcloud provider is properly working. For the .box file we will use a sample precise32 (Ubuntu 12.04 32bit) vagrant .box file created for the vcloud provider by Timo Sugliani: 

{% highlight bash %}
[root@vagrant ~]# mkdir -p ~/vagrant-vms/precise32-test
[root@vagrant ~]# cat ~/vagrant-vms/precise32-test/Vagrantfile 
precise32_vm_box_url = "http://vagrant.tsugliani.fr/precise32.box"

nodes = [
  { :hostname => "test-vm",  
    :box => "precise32", 
    :box_url => precise32_vm_box_url }
]

Vagrant.configure("2") do |config|

  # vCloud Director provider settings
  config.vm.provider :vcloud do |vcloud|

    vcloud.hostname = '[Youor_vCHS_vCloud_Director_API_URL]'
    vcloud.username = '[Your_vCHS_username@somedomain.com]'
    vcloud.password = '[Your_vCHS_password]'

    vcloud.org_name = '[Your_vCHS_org_name]'
    vcloud.vdc_name = '[Your_vCHS_vdc_name]'
    vcloud.catalog_name = '[Your_vCHS_org_catalog]'
    vcloud.network_bridge = true
    vcloud.vdc_network_name = '[Your_vCHS_vdc_network_name]'

  end

  nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.box = node[:box]
      node_config.vm.hostname = node[:hostname]
      node_config.vm.box_url = node[:box_url]
    end
  end
end
{% endhighlight %}

#### B.  Now we can change directory to that directory and test everything with a "vagrant up --provider=vcloud":

{% highlight bash %}
[root@vagrant ~]# cd /root/.vagrant.d/gems/gems/vagrant-vsphere-0.8.1/example_box
[root@vagrant example_box]# tar cvzf dummy.box ./metadata.json
./metadata.json
{% endhighlight %}

#### C.  Next we will create a new folder under /root for storing the box file we created and other vagrant configuration files

{% highlight bash %}
[root@vagrant example_box]# mkdir -p ~/vagrant-vms/example_box
[root@vagrant example_box]# mv dummy.box ~/vagrant-vms/example_box
{% endhighlight %}

#### D. Vagrant reads it's configuration from a file named Vagrantfile, located in the directory from which the "vagrant up" command is run in. We need to go back to our "vagrant-vms" folder and create this file:

{% highlight bash %}
[root@vagrant example_box]# cd ~/vagrant-vms/
[root@vagrant vagrant-vms]# touch Vagrantfile
{% endhighlight %}

#### E. Modify the Vagrantfile to reflect your vsphere configuration. Here's an example of my working Vagrantfile:

{% highlight bash %}
[root@vagrant vagrant-vms]# cat Vagrantfile

Vagrant.configure("2") do |config|
  config.vm.box = 'dummy'
  config.vm.box_url = './example_box/dummy.box'

  config.vm.provider :vsphere do |vsphere|
    vsphere.host = 'vcenter.mylab.net'
    vsphere.name = 'vagrant-test'
    vsphere.clone_from_vm = true
    vsphere.template_name = 'Centos-6.5'
    vsphere.user = 'root@localos'
    vsphere.password = 'S0meR@nd0mP@ssw0rd'
    vsphere.insecure = true
  end
end
{% endhighlight %}

In this configuration file:

* "vsphere.host" is your vCenter server FQDN or IP address
* "vsphere.name" will be the name of the virtual machine that gets created
* "vsphere.clone\_from\_vm" specifies we will be cloning a vm rather than template
* "vsphere.template_name" is the name of the vm we will be cloning
* "vsphere.user" is the username we will be using to connect to vCenter
* "vsphere.password" is the password we will be using to connect ot vCenter
* "vsphere.insecure" tells vagrant to not worry about validating the certificate on the vCenter server

### 9. Vargrant up!!!!
If everything has gone correctly up to this point we can issue a "vagrant up --provider=vsphere" command from the directory that contains the Vagrantfile:

{% highlight bash %}
[root@vagrant vagrant-vms]# vagrant up --provider=vsphere
Bringing machine 'default' up with 'vsphere' provider...
==> default: Calling vSphere CloneVM with the following settings:
==> default:  -- Source VM: CentOS-6.5
==> default:  -- Name: vagrant-test
==> default: Waiting for SSH to become available...
{% endhighlight %}

You should see a "Clone virtual machine" task being run on your vCenter server and then completing.

### 10. Canceling the "Vagrant up" command and destroying our cloned vm.
Since we have not properly prepared the vm we are cloning (I will have another blog post covering this topic) Vagrant will never be able to successfully connect to the cloned vm using SSH. As a result we will need to cancel the "vagrant up" command:

#### A. Press [control-c] to attempt to gracefully exit "vagrant-up"

{% highlight bash %}
^C==> default: Waiting for cleanup before exiting...
{% endhighlight %}

I've never seen this task complete so I will press [control-c] again to immediately exit:

{% highlight bash %}
^C==> default: Exiting immediately, without cleanup!
{% endhighlight %}

#### B. Destroy the cloned vm by running "vagrant destroy" command:

{% highlight bash %}
[root@vagrant vagrant-vms]# vagrant destroy
==> default: Calling vSphere PowerOff
==> default: Calling vShpere Destroy
{% endhighlight %}

You should see a "Power Off virtual machine" and "Delet virtual machine" tasks being run and then completing on your vCenter server.

### Hopefully you found this post helpful in getting vagrant and the vagrant-vsphere plugin installed and configured. The next blog post will be covering how to create a template vm that has been prepared for vagrant to connect seemlessly using SSH.

### Please provide any feedback or suggestions to my twitter account located on the about page.

