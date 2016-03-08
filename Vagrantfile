# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

# http://docs.ansible.com/ansible/guide_vagrant.html
Vagrant.require_version ">= 1.7.0"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box_check_update = true
  config.ssh.insert_key = false

  config.vm.define :wordpress_server do |node|
    node.vm.box = "bento/centos-6.7"

    node.vm.network :forwarded_port, guest: 80, host: 80
    node.vm.network :private_network, ip: "192.168.33.11"

    node.vm.provider :virtualbox do |vb|
      vb.memory = "2048"
      vb.cpus = "1"
    end

    node.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.extra_vars = "@credentials.yml"
    end
  end

  config.push.define :atlas do |push|
    push.app = "ymkjp/wordpress-base"
  end
end
