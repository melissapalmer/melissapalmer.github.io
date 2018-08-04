---
layout: post
title: "Vagrant AWS"
date: 2018-08-02 17:45:09 -0700
categories: docker
tags: 
- Vagrant
- AWS
description: Vagrant AWS
published: true
---


Vagrant ships out of the box with support for VirtualBox, Hyper-V, and Docker,
Before you can use another provider, you must install it. Installation of other providers is done via the Vagrant plugin system.

BUG that libxml2 is not found not found on Windows 10 for the vagrant-aws plugin, therefore have to run 2 commands (Windows 10, Vagrant Installed Version: 2.1.1) 
https://github.com/mitchellh/vagrant-aws/issues/539 comment from `rurku commented on Jun 18`

`vagrant plugin install --plugin-version 1.0.1 fog-ovirt`
`vagrant plugin install vagrant-aws`

`vagrant plugin list`

You should see 2 plugings 

```
fog-ovirt (1.0.1)
  - Version Constraint: 1.0.1
vagrant-aws (0.7.2)
```

add the dummy box using any name
`vagrant box add aws/dummy https://github.com/mitchellh/vagrant-aws/blob/master/dummy.box`

I needed to download the dummy.box locally and then run (in folder where dummy.box was saved) 
`vagrant box add aws/dummy dummy.box`

Run 
`vagrant box list`
to verify the box has been added 


You need to login to AWS account and gets some information 
https://console.aws.amazon.com/iam/home#security_credential

Access Key ID:		AKIAIK2JIHGBSH3LTVMA
Secret Access Key:	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

`vagrant up --provider=aws`


I kept Being asked for details for SMB Correct with:
```
config.vm.synced_folder ".", "/vagrant", disabled: true  
```

All seemed on BUT I got error
```
There was an error talking to AWS. The error message is shown
below:

OptInRequired => You are not subscribed to this service. Please go to http://aws.amazon.com to subscribe.
```
It seems you have to have entered you credit card details to use the AWS



References
===



- [https://www.vagrantup.com/intro/getting-started/providers.html](https://www.vagrantup.com/intro/getting-started/providers.html)
https://www.vagrantup.com/docs/providers/
- [https://devops.com/devops-primer-using-vagrant-with-aws/](https://devops.com/devops-primer-using-vagrant-with-aws/)

https://github.com/mitchellh/vagrant-aws/issues/539
https://github.com/mitchellh/vagrant-aws

https://blog.scottlowe.org/2016/09/15/using-vagrant-with-aws/
https://devops.com/devops-primer-using-vagrant-with-aws/

