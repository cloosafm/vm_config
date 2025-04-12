# VM creation

## What it does
At École 42, we often need to work in a VM. Also, making this config felt like a good occasion to learn more about vagrant and ansible !
So what do we have here ?
First, vagrant creates a minimal ubuntu22.04 box, using virtualbox as provider ; then ansible configures the VM to be suitable for most basic dev needs. It takes a few minutes depending on your system, and you can start using the box as soon as it has restarted with the GUI.

## How to use it
Prerequisite (minimal) :
- VirtualBox v7.x
- Vagrant v2.4.x
- Ansible v2.17.x

### VirtualBox Guest Additions
In order to use VirtualBox Guest Additions, make sure the plugin is installed :
```vagrant plugin install vagrant-vbguest```

### Vagrant configuration
Rename the confs/example_vars.yaml file as confs/vars.yaml and customize the values.
Then just do ```vagrant up``` and let the configuration run its full course (including the restart) before using the VM.

## Configuring the VM

### Current Ansible roles:

#### apt_basics
Perform apt update, then install basic tools for dev (ca-certificates, curl, vim, build-essential, etc).
Also creates a keyring.

#### remove_snap
Remove Snapd and all snap packages.
Snap is known to use a lot of resources, also this way we are sure to only install the packages we want.
And yes, I could have decided to just use another distro. But the goal was to provide a quick'n'easy way to spin up a VM for students of 42 school, and our school computers use Ubuntu. So this is just about providing a familiar environment to my peers.

#### security_tools
Install and enable UFW and OpenSSH.
Create one ed25519 ssh key with default values.

#### gui_install
Install a minimal Gnome.

#### web_tools
Install Firefox.

#### dev_tools
Ensure binutils, make, cmake, git (list in defaults/main.yml) are installed.
Install Python and pip, as well as VSCode.

#### docker_install
Install Docker, docker-compose and docker-cli.

#### vb_guest_additions
Install VirtualBox Guest Additions, to allow bidirectional clipboard and drag-and-drop.

#### reload_vm
To complete the installation of the GUI and VB Guest Additions, restarting the VM is necessary.

### Do you need more ?
If the provided roles are not sufficient, you can create your own :
```ansible-galaxy init <role_name>```

In the provided ansible.cfg, the roles are defined to be in the same directory as the cfg file.

## Additional information

This was tested on Ubuntu. On a different OS, you may need to modify certain paths, such as the local one for "config.vm.synced_folder" - for instance on Mac, it might be "/Applications/VirtualBox.app/Contents/MacOS"

## Future development
- The cursor shows as a white square. Adding a specific icon theme did not help. Will also try with another GUI.
- Although Ansible fact gathering is turned on, some facts/variables seem to not be collected.
- Overall (and over time), this project will benefit from a better understanding of ansible-galaxy possibilities.
