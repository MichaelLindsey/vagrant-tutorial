# Vagrant Development Environment

Hasicorp Vagrant is a Virtual Machine automation tool written in Ruby that can
be really useful for:

1. Keeping a consistent development environment that can be shared quickly
between users. Solve the "It works on my machine problem".
2. Automated installation of VM's. Vagrant takes care of the entire
installation and setup of your VM through Virtualbox.
3. Great Development tool for any infrastructure as code project running via
VirtualMachines, EC2 or Azure VM's.

### Vagrant - Dependencies

**Fedora**  
Packages are available via stock Fedora repositories.  
* VirtualBox [official site](https://www.virtualbox.org/)  
* Vagrant [official site](https://www.vagrantup.com/docs/installation/)

**Windows**  
Install Virtualbox & Vagrant manually or via the unoffical package manager: 
[chocolatey](https://chocolatey.org/)  
You may also need a proper SSH client, it worked fine for me with Powershell
in Windows10.

### Vagrant Plugins
Can be installed from the cli, I find the vbguest plugin that automatically
installs and updates VirtualBox Guest editions to be a mandatory plugin.  

Install vagrant-vbguest plugin: `vagrant plugin install vagrant-vbguest`  

### Vagrant Installation - without sudo rights.
Vagrant / Virtualbox requires a kernel modules package to be installed and
reloaded. If your are installing for the first time, you just need to reboot
your machine once it's installed and everything should work.

Common Errors:
```
VirtualBox is complaining that the kernel module is not loaded. Please
run `VBoxManage --version` or open the VirtualBox GUI to see the error
message which should contain instructions on how to fix this error.
```

run `VBoxManage --version` which will list 2 packages you need to install via
dnf, example below (the kernel module is likely different)
```
sudo dnf install kernel-modules-5.5.16-100.fc30.x86_64
sudo dnf install akmod-VirtualBox
```
I'd also recommend restart after the installation. I've had this fail me before
as well during an update of the tools. If that's the case remove Virtualbox,
Vagrant, re-install and reboot.

### Getting started
Presuming all the dependencies are installed, from the project root directory
type `vagrant up` to create the VM and run the test\_automation\_code.yml
ansible playbook. You can log into the VM with `vagrant ssh`, re-run the tests
with `vagrant up --provision`, shut it down `vagrant halt` and delete it to
start over with `vagrant destroy`.

### The Common Commands
Vagrant full documentation: <https://www.vagrantup.com/docs/cli/>  
`vagrant init` - Creates a blank Vagrantfile with commented options.  
`vagrant status` - Check status of VM, Running?, Shutdown etc?  
`vagrant up` - Creates VM or starts a halted one.  
`vagrant up --provision` - If VM is already running, this will re-run 
provisioners really handy for developing ansible playbooks.  
`vagrant reload` - Equivalent of vagrant halt and vagrant up, re-sync's
/vagrant folder.  
`vagrant halt` - Shutdown a VM.  
`vagrant destroy` - Delete a VM.  

### Misc. about Vagrant
**Boxes:** Hashicorp hosted or they can be self-hosted Virtual Machine images,
the template in this repository is configured to use a public centos/7 image.
You can search available images via 
[vagrant-box-search](https://app.vagrantup.com/boxes/search)

**The /vagrant share:** By default Vagrant rsync copies the launch directory
into into the VM under "/vagrant". If you make changes to the code on your
physical machine you will need to run `vagrant reload` to re-sync the to the
VM. It is possible to mount shares to the VM but it requires elevated
privileges.

The Included Vagrant file is commented and configured to allow easy testing of
Ansible codebase against a machine. At present it is configured to use a stock
CentOS7 image.

**Vagrantfile**
Link to the official documentation, the "config.vm" section contains most
relevant settings. <https://www.vagrantup.com/docs/vagrantfile/>  

A Vagrantfile is used to describe the type of machine required for a project,
the file in this repo is a little bit of a re-write of the standard Vagrantfile
generated by `vagrant init` to utilize Ruby functions and loops with the idea
of improving readability.

I've commented a number of sections in the file in this repository for common
options, like multiple machines, port forwarding.