# -*- mode: ruby -*-
# vi: set ft=ruby :

#####################################################################################
# Copyright 2012 Normation SAS
#####################################################################################
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, Version 3.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
#####################################################################################


Vagrant.configure("2") do |config|

  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Boot with a GUI so you can see the screen. (Default is headless)
  # config.vm.boot_mode = :gui

  # Share an additional folder to the guest VM. The first argument is
  # an identifier, the second is the path on the guest to mount the
  # folder, and the third is the path on the host to the actual folder.
  # config.vm.share_folder "v-data", "/vagrant_data", "../data"


  # VM declaration
  # name   : VM os name in vagrant
  # box    : Box name
  # url    : URL where to fetch the box
  # server : File to use as server provisionning script (they are in provision folder)
  # node   : File to use as node provisionning script (they are in provision folder)

  #################### SERVER BOXES ###########################

  debian7 = {
    :name   => "debian",
    :box    => "debian-7.0-amd64-minimal",
    :url    => "https://dl.dropboxusercontent.com/s/xymcvez85i29lym/vagrant-debian-wheezy64.box",
    :server => "server.sh",
    :node   => "node.sh"
  }

  sles11 = {
    :name   => "sles",
    :box    => "sles-11-64",
    :url    => "http://puppetlabs.s3.amazonaws.com/pub/sles11sp1_64.box",
    :server => "server_sles11.sh",
    :node   => "node_sles11.sh"
  }

  ubuntu12_04 = {
    :name   => "ubuntu",
    :box    => "ubuntu12.04",
    :url    => "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box",
    :server => "server_ubuntu.sh",
    :node   => "node_ubuntu.sh"
  }

  centos6 = {
    :name   => "centos",
    :box    => "centos6",
    :url    => "http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.3-x86_64-v20130101.box",
    :server => "server_centos6.sh",
    :node   => "node_centos6.sh"
  }


  #################### NODE BOXES ###########################

  debian6 = {
    :name   => "debian6",
    :box    => "debian-squeeze-64",
    :url    => "http://dl.dropbox.com/u/937870/VMs/squeeze64.box",
    :node   => "node.sh"
  }

  centos5 = {
    :name   => "centos5",
    :box    => "centos5",
    :url    => "http://www.lyricalsoftware.com/downloads/centos-5.7-x86_64.box",
    :node   => "node_centos5.sh"
  }

  os = [ debian6, debian7, sles11, ubuntu12_04, centos6, centos5]
  server = { :ip       => "192.168.42.10",
             :hostname => "server"
           }


  os.each { |os|
    # Declare server boxes if server provisionning script is declared
    if os[:server] 
      config.vm.define ("server_"+os[:name]).to_sym do |server_config|
        server_config.vm.box =  os[:box]
        server_config.vm.box_url = os[:url]
        server_config.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", "1536"]
        end
        server_config.vm.network :forwarded_port, guest: 80, host: 8080
        server_config.vm.network :forwarded_port, guest: 443, host: 8081
        server_config.vm.network :private_network, ip: server[:ip]
        server_config.vm.hostname = server[:hostname]
        server_config.vm.provision :shell, :path => "provision/"+os[:server]
      end
    end

    (1..10).each { |i|
      n = i.to_s()
      config.vm.define ("node"+n+"_"+os[:name]).to_sym do |node_config|
        node_config.vm.provision :shell, :path => "provision/"+os[:node]
        node_config.vm.network :private_network, ip: "192.168.42.1"+n
        node_config.vm.box =  os[:box]
        node_config.vm.box_url = os[:url]
        node_config.vm.provider :virtualbox
        node_config.vm.hostname = "node"+n
      end
    }
  }
end

