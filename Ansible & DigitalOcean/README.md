# Getting Started on Ansible and Digital Ocean 

For some reason I've recently been attracted to [DevOps](http://en.wikipedia.org/wiki/DevOps). I've done some work on [Puppet](http://puppetlabs.com/) and others, but I have found that I'm more productive (and enjoy) working with [Ansible](http://www.ansibleworks.com) (thanks to [@scottmote](https://twitter.com/scottmotte) for introducing it to me). With this blog-series I hope to get you up and running in Ansible by deploying and provisioning a [Digital Ocean](https://www.digitalocean.com/?refcode=4a034bcac721
) Droplet and deploying a [Go](http://golang.org) app.

First I would like to provide you with a simple gist of what Ansible and Digital Ocean are. Ansible is a easy-to-use tool that can execute pre-defined actions on servers such as installing dependencies, watching for changes, deploying applications or even deploying other servers. Digital Ocean is a cloud hosting provider that is super simple and cheap. 

![Ansible-Logo](http://ww1.prweb.com/prfiles/2013/06/11/11256896/AnsibleNoTag_CLR.jpg)

### Installing Ansible 

Ansible is dead simple to [install](http://www.ansibleworks.com/docs/intro_installation.html#latest-releases-via-pip). Just run:

	sudo pip install ansible
	
![DO-Logo](https://www.digitalocean.com/assets/v2/badges/digitalocean-vertical-eps.png)
	
### Set up Digital Ocean

Take the time to create an account in Digital Ocean. There are coupons out there. [Tweet me](https://twitter.com/elbuo8) if you need help finding them. 
After your account is up and running, lets add your SSH key to it. Doing so will save us the hassle of managing passwords for our VMs. Click the following link to add them. If you don't know how to create your SSH keys, click [here](https://www.digitalocean.com/community/articles/how-to-use-ssh-keys-with-digitalocean-droplets).

[Add keys to Digital Ocean](https://cloud.digitalocean.com/ssh_keys).

### Set up the Workspace

We will be adding our [DO credentials](https://www.digitalocean.com/community/articles/how-to-use-the-digitalocean-api) in order to use an Ansible [Plug-In](https://github.com/ansible/ansible/tree/devel/plugins/inventory) to interface with their API.

	mkidr ansible-tutorial
	cd ansible-tutorial
	export DO_API_KEY=YOUR DIGITAL OCEAN API KEY
	export DO_CLIENT_ID=YOUR DIGITAL OCEAN CLIENT ID
	sudo pip install dopy 
	touch hosts
	
### Creating a Droplet

In order to create a Droplet, we meed to fetch some things from the [DO API](https://cloud.digitalocean.com/api_access).

*	ID of the SSH Key previously stored
*	ID of Image to be used
*	ID of Region
*	ID of Size 

Since this might be a little of a hassle I wrote a script that will provide you with all the needed information.

	curl https://gist.github.com/elbuo8/7714478/raw/b047721b49f39967a47368cc905007b56e4631f7/doapi.sh | sh
	
Now add the following to your **hosts** file previously created.

	[localhost]
	localhost
The **hosts** file is our inventory. Ansible runs its Playbooks (pre-defined commands) over the servers which pattern is matched in the inventory.

Now on with the Playbook. Save this file as **newdroplet.yml**. Replace the values that you obtained before.

	---
	- hosts: localhost
  	  tasks:
  		- name: Create new DO Droplet
    	  digital_ocean: >
      	  state=present
      	  command=droplet
      	  name=ansible-tutorial
      	  size_id=SIZEID
      	  region_id=REGIONID
      	  image_id=IMAGEID
      	  ssh_key_ids=SSHKEYID

Almost done! Now all we have to do is run this command to create that Droplet.

	ansible-playbook newdroplet.yml -c local -i hosts
	
And just like that, you deployed a Droplet on DO using Ansible.