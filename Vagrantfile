# Vagrantfile template.
# The following template was written to simplify getting started with Hashicorp
# Vagrant and Virtualbox allowing a dev team to run and test code locally.
# Full documentation can be found https://www.vagrantup.com/docs/.

# Define VM Specifications.
def configure_vm_specs(config)
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', 1024]
    vb.customize ['modifyvm', :id, '--cpus', 2]
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--ioapic', 'on']
  end
end

# Define VM image and optional port forwards and system mounts.
def configure_centos(config)
    # Select An Image, default location https://app.vagrantup.com/boxes/search
    config.vm.box = "centos/7"
    # You can also host your own Vagrant box repository
    # config.vm.box_url = http:// or file://
    # Configure port forwarding  to the virtual machine.
    config.vm.network 'forwarded_port', guest: 8080, host: 8080, protocol: 'tcp'
    # Folders can be mount to the VM (req's Sudo so unavailable)
    # config.vm.synced_folder "./ansible", "/ansible"
end

# Vagrant supports many "provisioners", provisioners allow you to execute code
# when your machine is created and periodically as you develop your code.
# Below is an example of the Vagrant shell provisioner utilizing an in-line
# script to install a Conda environment and run an Ansible playbook.

$script = <<-SCRIPT

# You will need to refactor the below script to suit your needs. The below
# is an example set up to run tests against the "automation_code" repository.

echo "Hello World!"

## Environment Variables Required By Ansible.
#APPLICATION="un-used-yet"
#ENVIRONMENT="build"
#
## Configure automation_code project environment
#sudo yum install python3 -y
#if [ ! -f "get-pip.py" ]; then
#    curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
#    python get-pip.py
#fi
#if [ ! -f "miniconda.sh" ]; then
#    curl "https://repo.anaconda.com/miniconda/\
#    Miniconda3-latest-Linux-x86_64.sh" -o "miniconda.sh"
#    chmod a+x ./miniconda.sh && ./miniconda.sh -b -p /home/vagrant/miniconda3
#    source /home/vagrant/miniconda3/bin/activate
#    conda env create -f /vagrant/environment.yml
#    conda deactivate
#fi
#
#source /home/vagrant/miniconda3/bin/activate
#conda activate autocode_py3
#cd /vagrant/ansible/ && ansible-playbook test_automation_code.yml
SCRIPT

def configure_shell_provisioner(config)
  config.vm.provision "shell", inline: $script
end


# The executing part of the code, we have specified Vagrant to build to VM's
# one has the shell provisioner commented out, the other runs it.

Vagrant.configure(2) do |vagrant_config|
  vagrant_config.vm.define "centos7-1" do |config|
    configure_vm_specs(config)
    configure_centos(config)
    configure_shell_provisioner(config)
  end

  # Example run a second virtual machine.
  #vagrant_config.vm.define "centos7-2" do |config|
  #  configure_vm_specs(config)
  #  configure_centos(config)
  #  configure_shell_provisioner(config)
  #end
end

