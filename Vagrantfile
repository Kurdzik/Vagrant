VAGRANT_COMMAND = ARGV[0]

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "forwarded_port", guest: 8880, host: 8880
  config.vm.network "forwarded_port", guest: 8082, host: 8082
  config.vm.network "forwarded_port", guest: 8081, host: 8081

  
  config.vm.provider "virtualbox" do |v|
    v.memory = 10240
    v.cpus = 3
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/clone_roles.yml"
    ansible.extra_vars = {
      git_repository: "https://github.com/Kurdzik/Ansible_roles.git",
      git_branch: "master"
    }
  end
    
  config.vm.network "private_network", ip: "192.168.44.44"
  
  config.vm.provision "ansible_local" do |ansible|
      ansible.galaxy_role_file = 'requirements.yml'
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
      ansible.playbook = "playbooks/init.yml"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/infrastructure.yml"
    ansible.extra_vars = {
      git_repository: "https://github.com/Kurdzik/Infrastructure.git",
      git_branch: "master"
    }
  end

  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = 'panda'
  end
end
