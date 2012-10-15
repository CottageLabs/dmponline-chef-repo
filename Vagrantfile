#Vagrant file to provision DMPOnline on Virtual Box
#Martyn Whitwell 2012-10-15

# This script is built with CentOS 6.3 x86_64 minimal
# https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box
# Other boxes are listed at http://www.vagrantbox.es
# To add the centos6 box into your system, run
# $ vagrant box add centos6 https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box

Vagrant::Config.run do |config|
  config.vm.box = "centos6"
  config.vm.box_url = "https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box"
  
  config.vm.host_name = "dmponline-vagrant"
  config.vm.network :hostonly, "10.10.10.10"
#  config.vm.forward_port 22, 2210
#  config.vm.forward_port 80, 8080

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.run_list = [ "recipe[dmponline]" ]
    chef.node_name = "dmponline"
    chef.provisioning_path = "/etc/chef"
    chef.log_level = :info
  end
  
end