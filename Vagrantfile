$script_ansible = <<-SCRIPT
apt update && \
apt install -y software-properties-common && \
apt-add-repository --yes ppa:ansible/ansible && \
apt update && \
apt install -y ansible
SCRIPT

Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end
  
  config.vm.define "wordpress" do |m|
    m.vm.network "private_network", ip: "172.17.177.40"

    m.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu_trusty_wordpress"
    end

    m.vm.provision "shell", inline: "cat /vagrant/chave/id_ansible.pub >> .ssh/authorized_keys"
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.network "public_network", ip: "192.168.0.203", 
      bridge: "Qualcomm Atheros QCA9377 Wireless Network Adapter"
    
    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu_trusty_ansible"
    end 

    ansible.vm.provision "shell", inline: $script_ansible
    
    ansible.vm.synced_folder "./ansible_files", "/ansible_files",
      mount_options: ["fmode=666", "dmode=777"]
      
    ansible.vm.synced_folder "./chave", "/chave",
      mount_options: ["fmode=600", "dmode=777"]        
  end
end
