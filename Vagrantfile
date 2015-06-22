# -*- mode: ruby -*-
# vi: set ft=ruby :
# requires vagrant plugin install salty-vagrant-grains
# requires vagrant plugin install vmware_fusion


Vagrant.configure(2) do |config|

  config.vm.define "nagios" do |nagios|
    nagios.vm.box = "vagrant-rhel71-x64"
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
################################################################################
#
# VMware Config
#
################################################################################
    nagios.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = 512
    end
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
################################################################################
#
# Network Config
#
################################################################################
    nagios.vm.hostname = "nagios.labs.oaksec.io"

    # Default SSH port should be 22
    # However some salt states might change this port
    # Update as needed for your lab
    nagios.ssh.port = 22
    nagios.vm.network "forwarded_port",
              guest: 80,
              host: 8080,
              auto_correct: true
    nagios.vm.network "private_network",
              ip: "192.168.211.60",
              netmask: "255.255.255.0"

    # Required for CentOS 7/RHEL and a private_network static IP
    nagios.vm.provision "shell",
              inline: <<-SHELL
      sudo systemctl restart NetworkManager
      sudo systemctl restart network
    SHELL
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
################################################################################
#
# RHEL Config (If box is RHEL)
#
################################################################################
    nagios.vm.provision "shell",
              run: "always",
              inline: <<-SHELL
      if grep -q 'ID="rhel"' "/etc/os-release"; then
        sudo subscription-manager repos --enable rhel-7-server-optional-rpms
        sudo subscription-manager repos --enable rhel-7-server-extras-rpms
      fi
    SHELL
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
################################################################################
#
# Salt Config
#
################################################################################
    nagios.vm.provision :salt do |config|
      config.install_type = "stable"
      config.bootstrap_options = ""
      config.temp_config_dir = "/tmp"
      config.minion_config = "etc/salt/minion"
      config.verbose = false
      config.run_highstate = false
    end
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
################################################################################
#
# Nagios Config
#
################################################################################
    nagios.vm.provision "shell",
              path: "install-specific-directory.sh"
  end
end
