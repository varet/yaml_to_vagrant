# Maintained by Billias 

# vi: set ft=ruby: 

#Prereqs
Vagrant.require_version ">= 1.6.0"
Vagrantfile_Version = "2"

require 'yaml'

# Read YAML file with box details
servers = YAML.load_file('servers.yaml')

#Set variables
suffix = servers["defaults"]["fqdn_suffix"]


# Create boxes
Vagrant.configure(Vagrantfile_Version) do |create|
  # Configure servers based on vms hash found in yaml
  servers["vms"].each do |servers|
    create.vm.define servers["name"] do |srv|

      name_of_vm = servers["name"] + "." + suffix

      srv.vm.box = servers["box"]
      srv.vm.network servers["network"], ip: servers["ip"]
      srv.vm.host_name = name_of_vm
      
      # Setup Memory and other parameters for your vm (also check `VBoxManage controlvm`)
      srv.vm.provider :virtualbox do |vb|
        vb.memory = servers["ram"]
        vb.cpus   = servers["cpus"]   if servers["cpus"]
        if servers["controlvm"] then
          servers["controlvm"].each do |dev, devvalue|
            controlvmcommand = ['controlvm', :id, dev] + devvalue
            vb.customize controlvmcommand
          end
        end
      end
    end
  end
end

