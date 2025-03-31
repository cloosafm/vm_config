# Vagrant version : 2.4.3

# For use with a variable config file:
require 'yaml'

# Specify the path to the config file
config_file = "confs/vars.yaml"

# Check if the config file exists; if not, raise an error
unless File.exist?(config_file)
  raise "Configuration file '#{config_file}' is missing! Exiting..."
end

# Load the configuration from conf.yaml
config = YAML.load_file(config_file)

box_name = config['box_name']
host_name = config['host_name']
vm_provider = config['vm_provider']
vm_box = config['vm_box']

vm_mem = config['vm_mem']
vm_cpu_count = config['vm_cpu_count']
vm_time_out = config['vm_time_out']

vm_gui = config['vm_gui']
vm_path = config['vm_path']
guest_add_path = config['guest_add_path']

#############################################

# Vagrant config

Vagrant.configure("2") do |config|
  config.vm.define box_name do |my_vm|
    my_vm.vm.box = vm_box
    my_vm.vm.hostname = host_name
    my_vm.vbguest.auto_update = true
    config.vm.synced_folder "/usr/share/virtualbox/", "/vagrant/guest_additions"

    # if you prefer script provisioning, use below line and comment out the Ansible provisioning block
    # my_vm.vm.provision "shell", privileged: true, path: <path/to/script>  # of course, the path can be a variable added to your vars.yam;

    # Ansible provisioning block
    my_vm.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/playbook.yaml"  # Path to the playbook you want to run
        ansible.config_file = "ansible/ansible.cfg"  # Optional: Specify if using a custom Ansible config file
        ansible.extra_vars = {
          "ansible_roles_path" => "./ansible/roles",  # Path to the roles directory
          "guest_add_path" => guest_add_path  # Pass the guest_add_path variable to Ansible
        }
      end


    config.vm.provider vm_provider do |vb|
      vb.gui = vm_gui
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"] # Enable clipboard sharing
      vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"] # Enable drag and drop
      vb.customize ["setproperty", "machinefolder", vm_path]
      vb.memory = vm_mem
      vb.cpus = vm_cpu_count
    end
  end
  
  config.vm.boot_timeout = vm_time_out

end
