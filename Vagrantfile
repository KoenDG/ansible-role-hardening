Vagrant.configure("2") do |config|
  config.vbguest.installer_options = { allow_kernel_upgrade: true }
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
    vb.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
  end

  config.vm.define "bullseye_vlan" do |bullseye_vlan|
    bullseye_vlan.vm.box = "debian/bullseye64"
    bullseye_vlan.ssh.insert_key = true
    bullseye_vlan.vm.hostname = "bullseye-vlan"
    bullseye_vlan.vm.boot_timeout = 600
    bullseye_vlan.vbguest.auto_update = false
    bullseye_vlan.vm.provision "shell",
      inline: "ip link set dev eth0 down; ip link set eth0 name eth0.101; ip link set dev eth0.101 up; dhclient -r eth0.101; dhclient eth0.101"
    bullseye_vlan.vm.provision "shell",
      inline: "apt-get update && apt-get -y install python3-pip && python3 -m pip install ansible"
    bullseye_vlan.vm.provision "ansible" do |a|
      a.verbose = "v"
      a.limit = "all"
      a.playbook = "tests/test.yml"
      a.extra_vars = {
        "ansible_become_pass" => "vagrant",
        "ansible_python_interpreter" => "/usr/bin/python3",
        "sshd_admin_net" => ["0.0.0.0/0"],
        "sshd_allow_groups" => ["vagrant", "sudo", "debian", "ubuntu"],
        "system_upgrade" => "false",
        "manage_aide" => "false",
      }
    end
  end

  config.vm.define "bullseye" do |bullseye|
    bullseye.vm.box = "debian/bullseye64"
    bullseye.ssh.insert_key = true
    bullseye.vm.hostname = "bullseye"
    bullseye.vm.boot_timeout = 600
    bullseye.vbguest.auto_update = false
    bullseye.vm.provision "shell",
      inline: "apt-get update && apt-get -y install python3-pip && python3 -m pip install ansible"
    bullseye.vm.provision "ansible" do |a|
      a.verbose = "v"
      a.limit = "all"
      a.playbook = "tests/test.yml"
      a.extra_vars = {
        "ansible_become_pass" => "vagrant",
        "ansible_python_interpreter" => "/usr/bin/python3",
        "sshd_admin_net" => ["0.0.0.0/0"],
        "sshd_allow_groups" => ["vagrant", "sudo", "debian", "ubuntu"],
        "system_upgrade" => "false",
     }
    end
  end

  config.vm.define "focal" do |focal|
    focal.vm.box = "ubuntu/focal64"
    focal.ssh.insert_key = true
    focal.vm.hostname = "focal"
    focal.vm.boot_timeout = 600
    focal.vm.provision "shell",
      inline: "apt-get update && apt-get -y install python3-pip && python3 -m pip install ansible"
    focal.vm.provision "ansible" do |a|
      a.verbose = "v"
      a.limit = "all"
      a.playbook = "tests/test.yml"
      a.extra_vars = {
        "sshd_admin_net" => ["0.0.0.0/0"],
        "sshd_allow_groups" => ["vagrant", "sudo", "ubuntu"],
        "ansible_python_interpreter" => "/usr/bin/python3",
      }
     end
   end

  config.vm.define "jammy" do |jammy|
    jammy.vm.box = "ubuntu/jammy64"
    jammy.ssh.insert_key = true
    jammy.vm.hostname = "jammy"
    jammy.vm.boot_timeout = 600
    jammy.vm.provision "shell",
      inline: "apt-get update && apt-get -y install python3-pip && python3 -m pip install ansible"
    jammy.vm.provision "ansible" do |a|
      a.verbose = "v"
      a.limit = "all"
      a.playbook = "tests/test.yml"
      a.extra_vars = {
        "sshd_admin_net" => ["0.0.0.0/0"],
        "sshd_allow_groups" => ["vagrant", "sudo", "ubuntu"],
        "ansible_python_interpreter" => "/usr/bin/python3",
      }
     end
   end

  config.vm.define "almalinux" do |almalinux|
    almalinux.vm.box = "almalinux/8"
    almalinux.ssh.insert_key = true
    almalinux.vbguest.auto_update = false
    almalinux.vm.provider "virtualbox" do |c|
      c.default_nic_type = "82543GC"
      c.memory = 2048
    end
    almalinux.vm.hostname = "almalinux"
    almalinux.vm.provision "shell",
      inline: "dnf clean all && dnf install -y python3-pip && python3 -m pip install -U pip && python3 -m pip install ansible"
    almalinux.vm.provision "ansible" do |a|
      a.verbose = "v"
      a.limit = "all"
      a.playbook = "tests/test.yml"
      a.extra_vars = {
        "sshd_admin_net" => ["0.0.0.0/0"],
        "sshd_allow_groups" => ["vagrant", "sudo"],
        "ansible_python_interpreter" => "/usr/bin/python3",
      }
    end
  end
end
