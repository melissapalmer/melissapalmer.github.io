---
layout: post
comments: true
title: "Using Vagrant (on Windows) to launch an AWS instance"
date: 2018-08-16 08:45:09 -0700
categories: devops
tags: 
- Vagrant
- AWS
description: Using Vagrant (on Windows) to launch an AWS instance
published: true
---

# Overview
In this post I will be Using Vagrant (on Windows) to launch an AWS instance. 
> While Vagrant ships out of the box with support for VirtualBox, Hyper-V, and Docker, Vagrant has the ability to manage other types of machines as well. This is done by using other providers with Vagrant.

The [Vagrant AWS Provider](https://github.com/mitchellh/vagrant-aws) can be used to Boot instances in AWS.

# You'll need 2 things installed:
- [Vagrant](https://www.vagrantup.com/downloads.html)		VMs initially have nothing installed on them. You need to install and configure them as if they were new computer. This is known as provisioning. Vagrant combines the powers of a provisioner and VirtualBox to configure VMs for you. (alternatives for provisioning are Ansible, Chef, Puppet)
- [PuTTY and PuTTYGen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)	ssh client (to login to Linux VM and generator of security keys)
All of these install like any other windows program.

# You'll need to create and setup an AWS Account:
## Ensure you have an AWS Account
You  an register for an [AWS Account](https://portal.aws.amazon.com/billing/signup#/support), choose the Basic Plan ("Free"). You will need to supply your credit card details :-( even though its a free option. 

## Setup an access key in AWS
Go to the [IAM console](https://console.aws.amazon.com/iam/home#/users), and add a new user. 
Ensure this user has _Programmatic Access_ and add it to a new group you call admin with AdministratorAccess. 
(The AWS wizard will step you through the above)

You'll then be provided with the Access Key ID and the Secret Access Key. You need to keep these somewhere safe, they wont be provided again in the AWS screens. 

## Create a new Key Pair

### First create SSH keys using PuTTYgen
To do this Digital Ocean has detailed instructions on how to do this at [How to Create SSH Keys with PuTTY on Windows](https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/create-with-putty/)). 

*Basically you need to:*

- Open PuTTYgen
- Click the Generate Button 
- Move the mouse over the blank area of PuTTYgen
- When the key is generated it will display in text box for you	
- Click 'Save Public Key' to save public key
- Click 'Save Private Key' to save private key (you will get a .ppk file (which apparently is a putty-specific format))
- From the .ppk file you need to "Extract the private key using puttygen" by
-- using PuTTYgen to load the ppk, then go to Conversion -> Export OpenSSH key. And save file with extension for this file is .pem.

*NOTE:* DON'T include a passphrase as Vagrant-aws currently fails if the private_key file has a passphrase.

### In AWS

- Go to the [EC2 console](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#KeyPairs:sort=keyName) 
- Under menu on the right : NETWORKING & SECURITY -> Key Pairs
- Click Import Key and upload the public key created above

The image below shows doing this
![AWS KeyPair](/assets/images/devops/AWS-KeyPair.JPG)

## Setup a Security Group in AWS
You will need to allow an SSH connection to the new instance that gets created. 

- Go to the [EC2 console](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#SecurityGroups:sort=groupId) 
- Under menu on the right : NETWORKING & SECURITY -> -> Security Groups
- Create a new group, and add a Rule for SHH as in the image below

![AWS SecurityGroup](/assets/images/devops/AWS-SecurityGroup.JPG)

## Choose an Image (to use as the base of machine/instance you spinning up)

You will see later in this post, that in the Vagrantfile you specify a ami (Amazon Machine Image). 
> An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance.  

- The IDs are region-specific, so you need to find one in the region you are using. The easiest way I found to find one is to start Launching an Instance via the AWS UI. 
- Go to EC2 console and click the [Launch instance](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:) button.

![AWS AMI Selection](/assets/images/devops/AWS-AMI.JPG)

# Prepare Vagrant

## Install the AWS provider plugin
Vagrant does not include AWS as a built in provider, you need to manually install this provider as a plugin. 

I had a couple of issues on Windows 10, with the standard vagrant-aws plugin. BUG that libxml2 is not found not found on Windows 10 for the vagrant-aws plugin, therefore have to run 2 commands (Windows 10, Vagrant Installed Version: 2.1.1) https://github.com/mitchellh/vagrant-aws/issues/539 comment from "rurku commented on Jun 18"

`vagrant plugin install --plugin-version 1.0.1 fog-ovirt`

`vagrant plugin install vagrant-aws`

To check if the plugins are installed correctly you can run 

`vagrant plugin list`

You should see 2 plugins 

```
fog-ovirt (1.0.1)
  - Version Constraint: 1.0.1
vagrant-aws (0.7.2)
```

## Add the dummy box using any name
Even through we using AWS which uses Amazon Machine Images (as mentioned above), the definition of a Vagrant box is mandatory in the Vagrantfile. 
The author of the vagrant-aws plugin created a dummy-box, that can used for this. To add this box to Vagrant use:

Download the box from: https://github.com/mitchellh/vagrant-aws/blob/master/dummy.box to your own PC. Then run (in folder where dummy.box was saved): 
`vagrant box add aws/dummy dummy.box`

Run 
`vagrant box list`
to verify the box has been added 

## Setup environment variables for AWS_ACCESS_KEY & AWS_SECRET_KEY
You don't want to include the AWS_ACCESS_KEY & AWS_SECRET_KEY in any files you commit to GIT. I created a batch file (setenv.bat) with the following lines (that I don't add to GIT) 



# References
- [https://www.vagrantup.com/docs/providers/](https://www.vagrantup.com/docs/providers/)
- [https://www.vagrantup.com/intro/getting-started/providers.html](https://www.vagrantup.com/intro/getting-started/providers.html)
- [https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/create-with-putty/](https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/create-with-putty/)
- [https://dev.vividbreeze.com/installing-jenkins-in-aws-using-vagrant-and-ansible/](https://dev.vividbreeze.com/installing-jenkins-in-aws-using-vagrant-and-ansible/)
- [https://github.com/mitchellh/vagrant-aws](https://github.com/mitchellh/vagrant-aws)
- [https://github.com/mitchellh/vagrant-aws/wiki/Common-Pitfalls](https://github.com/mitchellh/vagrant-aws/wiki/Common-Pitfalls)
- [https://github.com/mitchellh/vagrant-aws/issues/539](https://github.com/mitchellh/vagrant-aws/issues/539)
- [https://rbgeek.wordpress.com/2015/02/19/provision-configure-ec2-instance-with-vagrant-and-ansible/](https://rbgeek.wordpress.com/2015/02/19/provision-configure-ec2-instance-with-vagrant-and-ansible/)



