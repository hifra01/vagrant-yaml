# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

yaml_configs = YAML.load_file(File.join(File.dirname(__FILE__), 'vagrant-config.yml'))
machines = yaml_configs['machines']
ssh_pub_keys = yaml_configs['ssh_public_keys']

Vagrant.configure("2") do |config|
  
  machines.each do |machine|
    
    config.vm.define machine['name'] do |srv|
      srv.vm.hostname = machine['name']
      srv.vm.box = machine['box']
      srv.vm.box_check_update = false

      if machine['sync_folder'] != nil
        srv.vm.synced_folder '.', '/vagrant', disabled: !machine['sync_folder']
      else
        srv.vm.synced_folder '.', '/vagrant', disabled: true
      end

      machine['nics'].each do |nic|
        if nic['ip_addr'] == 'dhcp'
          srv.vm.network nic['type'], type: nic['ip_addr']
        else
          srv.vm.network nic['type'], ip: nic['ip_addr']
        end
      end

      srv.vm.provider 'vmware_workstation' do |vmw|
        vmw.vmx['memsize'] = machine['ram']
        vmw.vmx['numvcpus'] = machine['vcpu']
        if machine['nested'] == true
          vmw.vmx['vhv.enable'] = 'TRUE'
        end
      end

      srv.vm.provider 'virtualbox' do |vb, override|
        vb.memory = machine['ram']
        vb.cpus = machine['vcpu']
      end
    end

    if ssh_pub_keys != nil
      ssh_pub_keys.each do |ssh_pub_key|
        config.vm.provision "shell" do |s|
          s.inline = <<-SHELL
            if grep -sq "#{ssh_pub_key}" /home/vagrant/.ssh/authorized_keys; then
              echo "SSH keys already provisioned."
              exit 0;
            fi
            echo "SSH key provisioning."
            mkdir -p /home/vagrant/.ssh/
            touch /home/vagrant/.ssh/authorized_keys
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
            chown -R vagrant:vagrant /home/vagrant
            exit 0
          SHELL
        end
      end
    end
  end
end
