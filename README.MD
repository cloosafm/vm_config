# VM creation

## How to use it
Prerequisite:
	- VirtualBox v7.x
	- Vagrant v2.4.x
	- Ansible


### Vagrant configuration
Rename the confs/example_vars.yaml file as confs/vars.yaml and customize the values.
Then just do ```vagrant up``` and let the configuration run its full course (including reloading the VM) before using the VM.

In order to use VirtualBox Guest Additions, make sure the plugin is installed :
```vagrant plugin install vagrant-vbguest```

Maybe all you want is a basic Vagrantfile with variables? Then comment out the ansible block.
Also if you prefer using a simple shell script instead of a full Ansible configuration, uncomment the appropriate line.



## Ansible configuration
The roles are defined as such:
- apt_basics		: install some basic packages, needed for several of the following roles
- remove_snap		: snap is known to use a lot of resources, also this way we are sure to only install the packages we wanr
- security_tools	: mainly ensure we have packages such as openssh, ufw. also creates an ssh key of type ed25519 with defaults
- gui_install		: install a minimal gnome
- web_tools			: to install firefox
- dev_tools			: VSCode, python, binutils
- docker_install	: docker
- reload_vm			: needed for the GUI


### About Ansible roles:
If the provided roles are not sufficient, you can create your own :
```ansible-galaxy init <role_name>```

In the provided ansible.cfg, the roles are defined to be in the same directory as the cfg file.

