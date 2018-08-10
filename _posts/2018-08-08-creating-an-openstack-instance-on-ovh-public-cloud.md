---
layout: post
title: Creating an openstack instance on OVH public cloud 
---

This is the first in a series of posts that will walk you through using the openstack-based OVH public cloud. Over the last few years, I have worked primarily with VMware products and have not taken the time to learn with openstack. Now that the OVH public cloud has launched in the United States, and I work for OVH US, I figured it was time to start learning how customers could put this offering to use.

I will attempt in this post to present the options that are available to OVH public cloud customers along side the choices I made that were specific to my instance.

Let's get started...

---

## 1. Create an account at [ovhcloud.com](https://ovhcloud.com/auth/).

The first thing you will need to do before you can create OVH public cloud instances is setup an account by going to [https://ovhcloud.com/auth/](https://ovhcloud.com/auth/) and clicking the 'Create an account' button. 

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-01-create-an-account.png)

After providing my email address, specifying a complex password and adding my billing address I received an email within minutes that my account had been activated.

## 2. Setup two-factor authentication.

The second thing you should do after creating an account is setup two-factor authentication. Enabling two-factor authentication will require that you provide a code (token) from your two-factor authentication application on your phone or physical device, along with your username and password when you log into ovhcloud.com. I would highly recommend setting up two-factor authentication to help prevent unauthorized access to your OVH account.

I found the option to enable two-factor authentication by going to My Account | Security | Two-factor authentication | activate double authentication

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-02-activate-two-factor-auth.png)

OVH supports mobile applications and physical devices for two-factor authentication. I selected mobile application since I would be using Google Authenticator.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-04-activate-two-factor-auth.png)

Next I scanned the QR code on my phone, provided a code (token) generated by the Google Authenticator app and gave it a friendly name.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-05-activate-two-factor-auth.png)

After clicking next you will be given emergency codes that can be used if you lose your mobile app or physical two-factor device. Copy these emergency codes and store them in a safe place since they can never be retrieved again. After clicking next the two-factor authentication option will show as being enabled.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-06-activate-two-factor-auth.png)

## 3. Adding a payment method.

You will now need to add a payment method, since it will be used to purchase cloud credits in the next step. I added a payment method by clicking my username at the upper right, clicking 'My payment methods' and finally clicking the 'Add a payment method' button. Provide your card number, experation date and security code and click the 'Add' button.

Once your payment method shows a "Confirmed" status you are ready to proceed to the next step.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-07-add-payment-method.png)

## 4. Creating a public cloud project.

The next step you need to do is create a cloud project. To do this I went to order dropdown and clicked 'Cloud project'.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-08-create-cloud-project.png)

Name your project and click 'launch your cloud project'.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-09-create-cloud-project.png)

On the next screen you will be purchasing credits that your running instances will consume.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-10-create-cloud-project.png)

Accept the terms and conditions by clicking the check box. Your cloud project will be ordered and payment method charged once you click the 'Validate and pay' button. 

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-11-create-cloud-project.png)

I received an email from OVH within 5 minutes of ordering to tell me that my cloud project had been created.

## 5. Creating a new ssh key for OVH public cloud to use.

You will need to generate a ssh key to connect to the OVH openstack instances. On my mac laptop I ran the following command to generate a new ssh key for this purpose.
{% highlight bash %}
ssh-keygen -b 4096 -t rsa -C "stan.dorsett+ovhus_public_cloud@gmail.com" -f ~/.ssh/id_rsa-ovhus_public_cloud
{% endhighlight %}

 - The `-b` option will specify the number of bits in the ssh key.
 - The `-t` option with specify the type of ssh key generated.
 - The `-C` option will add a comment to the end of the public key file.
 - The `-f` option will specify the file that the private key will be saved to. 

When running the command you will also get asked you if you want to add a passphrase that will need to be provided when you use the ssh key. I didn't choose to set a passphrase, but you can choose to if you feel the need.

Here is the output from my running of this command.

{% highlight bash %}
MacBook-Pro:~ standorsett$ ssh-keygen -b 4096 -t rsa -C "stan.dorsett+ovhus_public_cloud@gmail.com" -f ~/.ssh/id_rsa-ovhus_public_cloud
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/standorsett/.ssh/id_rsa-ovhus_public_cloud.
Your public key has been saved in /Users/standorsett/.ssh/id_rsa-ovhus_public_cloud.pub.
The key fingerprint is:
SHA256:7kSErA6S8zCIq0H/2hmcxE4+JUsW6ZmoDPIE/ULM8I4 stan.dorsett+ovhus_public_cloud@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|.                |
| *   ...         |
|. *  oo .        |
|o* .+.+.         |
|E.=.o@ .S        |
|=Xo+O =o         |
|ooo..O  o        |
|..  o +o         |
|.  ..+  .        |
+----[SHA256]-----+
MacBook-Pro:~ standorsett$
{% endhighlight %}

Output the public key so you can use it in the next step.

{% highlight bash %}
MacBook-Pro:~ standorsett$ cat /Users/standorsett/.ssh/id_rsa-ovhus_public_cloud.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDJ8LcFN8MRO/uwgVcdDg0grv
...lots of more characters here...
l0nXM7jjnjD9L3UMFw== stan.dorsett+ovhus_public_cloud@gmail.com
MacBook-Pro:~ standorsett$
{% endhighlight %}

## 6. Add the ssh pub key to the OVH public cloud.

You next need to add the public key generate in the previous step to OVH public cloud so it can be used to connect to instances you create.

Go to the project you created in the previous step. You can find this by clicking Cloud | Servers | [your project name].

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-12-view-cloud-project.png)

Click the 'SSh keys' tab to view or create new ssh keys.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-13-add-ssh-key.png)

Click 'Add a new key' to add the public key generated in the previous step.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-14-add-ssh-key.png)

Paste the output from the public key file, provide a friendly name for the ssh key and click "Add this key.'

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-15-add-ssh-key.png)

## 7. Create a new openstack instance.

Under your cloud project and click the Infrastructure tab. From the Actions dropdown click 'Add a server.'

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-16-add-a-server.png)

Choose the OS (image) that you want to have deployed.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-17-pick-an-os-image.png)

Select the server type (flavor) of the instance you want to create. The server type will specify the number of vcpu, memory, disk size and price per hour of the openstack instance that will be created:

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-18-select-instance-type.png)

Select the ssh key you created in the previous step. Click the 'Launch now' button to create the instance.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-19-launch-instance.png)

Once the instance is created a 'Login information' window will be displayed with the ssh username and ip address you will need to connect to the instance.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-20-login-info.png)

## 8. SSH to the new openstack instance,

You can now use the login infomation provided in the last step along with the private ssh key you generated to connect to your new instance. I used the following commands to ssh from my Mac laptop to connect.

{% highlight bash %}
ssh -i /Users/standorsett/.ssh/id_rsa-ovhus_public_cloud centos@147.135.76.67
{% endhighlight %}
 - The `-i` option is used to specify the ssh private key you generated earlier.

Here is the output from my running of this command.

{% highlight bash %}
MacBook-Pro:~ standorsett$ ssh -i /Users/standorsett/.ssh/id_rsa-ovhus_public_cloud centos@147.135.76.67
The authenticity of host '147.135.76.67 (147.135.76.67)' can't be established.
ECDSA key fingerprint is SHA256:aVSsv0mHKgIizglcvouJkvtBBOiZfNcQTpIc5oxhNO4.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '147.135.76.67' (ECDSA) to the list of known hosts.
[centos@server-1 ~]$
{% endhighlight %}

You can now run what commands you would like on your new openstack instance, like checking the IP address of the instance you created.

{% highlight bash %}
[centos@server-1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether fa:16:3e:42:97:c4 brd ff:ff:ff:ff:ff:ff
    inet 147.135.76.67/32 brd 147.135.76.67 scope global dynamic eth0
       valid_lft 86350sec preferred_lft 86350sec
    inet6 fe80::f816:3eff:fe42:97c4/64 scope link
       valid_lft forever preferred_lft forever
[centos@server-1 ~]$ 
{% endhighlight %}

## 9. Destroy the openstack instance.

Once you are finished with your openstack instance, you should delete the instance in order to stop being charged for it's usage. Click the drop down on the instance and select 'delete' to delete the instance.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-21-instance-options.png)

Click 'Confirm' to confirm that you want to delete the instance. This is your last chance to reconsider if you really want to delete it.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-22-confirm-delete.png)

Your cloud project will now be back to not having any server instances.

![screenshot]({{https://sdorsett.github.io }}/assets/08082018-23-empty-cloud-project.png)

---
###That all for this initial post covering how to sign up and get started using the OVH public cloud. 

###Please provide any feedback or suggestions to my twitter account located on the about page.