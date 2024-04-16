# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

yaml_configs = YAML.load_file(File.join(File.dirname(__FILE__), 'vagrant-config.yml'))
machines = yaml_configs['machines']

Vagrant.configure("2") do |config|
  
  machines.each do |machine|
    
    config.vm.define machine['name'] do |srv|
      srv.vm.hostname = machine['name']
      srv.vm.box = machine['box']

      if machine['sync_folder'] != nil
        srv.vm.synced_folder '.', '/vagrant', disabled: !machine['sync_folder']
      else
        srv.vm.synced_folder '.', '/vagrant', disabled: true
      end

      machine['nics'].each do |nic|
        if net['ip_addr'] == 'dhcp'
          srv.vm.network net['type'], type: net['ip_addr']
        else
          srv.vm.network net['type'], ip: net['ip_addr']
        end
      end

end
