DMPOnline + Sword "Getting Started" Chef Repository
===========================================

Introduction
------------
This Chef repository allows you to quickly build and provision an instance of [DMPOnline](https://github.com/CottageLabs/dmponline) + [Simple Sword Server](https://github.com/swordapp/Simple-Sword-Server) on a virtual server, either running locally using Virtual Box or on the cloud (e.g. via OpenStack, Rackspace or AWS).

Requirements
------------
In order to run the Chef recipe, you will need:

* [Ruby 1.9+](https://rvm.io/rvm/install/) (RVM is recommended)
* Rubygems (comes with RVM)
* [Git](https://help.github.com/articles/set-up-git)
* [Chef Client / Chef Solo](http://wiki.opscode.com/display/chef/Installing+Chef+Client+and+Chef+Solo) (`$ gem install chef`)
* [Librarian Chef](https://github.com/applicationsonline/librarian) (`$ gem install librarian`)

In addition you may also need:

* An [OpenStack](http://www.openstack.org) account and node, if deploying to the OpenStack cloud
* An [OpsCode](http://www.opscode.com) account, if provisioning with Hosted Chef
* [Oracle Virtual Box](https://www.virtualbox.org), if deploying to a local virtual machine
* [Vagrant](http://vagrantup.com), if deploying to a local virtual machine
* A [Centos 6 base box](http://www.vagrantbox.es), if deploying to a local virtual machine with Vagrant and Virtual Box


Deploying to a local Virtual Box with Vagrant and Chef Solo
-----------------------------------------------------------
1. First of all, make sure you have successfully installed on the prerequisite software: *Ruby*, *Gem*, *Git*, *Chef Solo*, *Librarian Chef*, *Virtual Box* and *Vagrant* (see links above). If you have installed software, its a good idea to reload open terminal windows to ensure all the new paths are added to your environment.

2. Clone this Chef repository onto your computer

		mkdir ~/projects
		cd ~/projects
		git clone https://github.com/CottageLabs/dmponline-chef-repo.git dmponline-chef-repo

3. Install the cookbooks with Librarian Chef

		cd ~/projects/dmponline-chef-repo
		librarian-chef install

4. Review the Vagrantfile with your favourite text editor. Pay special attention to the base box used (e.g. `centos63_minimal` and its associated URL), and also to the IP port mappings assigned to the virtual machine (e.g. http://localhost:80/ and http://localhost:8080/). Adjust these settings to suit your requirements and base box availability. See [www.vagrantbox.es](http://www.vagrantbox.es) for a list of base boxes. A minimal Centos 6.3 is recommended, but, with tweaking, the recipe should work with most versions of Linux.

		vi Vagrantfile

5. With the Vagrantfile configured, its time to download the base box, provision the server and install DMPOnline. This process will take some time to run, depending on your network and computer speed. Allow around 30-60 minutes to run.

		vagrant up

6. If the command completed successfully, open up a web browser and navigate to [http://localhost:80/](http://localhost:80) (or whatever you set for the IP address/port mapping in the Vagrantfile). If everything worked, you should see the DMPOnline welcome page!

 ![DMPOnline screen shot](https://raw.github.com/CottageLabs/dmponline-chef-repo/master/images/dmponline.png "DMPOnline screen shot")

 You should also see Simple Sword Server running on the other address mapped in the Vagrantfile, [http://localhost:8080/](http://localhost:8080) (NB. Simple Sword Server assumes it will be running at http://localhost:8080/)

 ![Simple Sword Server screen shot](https://raw.github.com/CottageLabs/dmponline-chef-repo/master/images/simple-sword-server.png "Simple Sword Server screen shot")


Deploying to OpenStack using Hosted Chef
----------------------------------------
1. First of all, make sure you have successfully installed on the prerequisite software: *Ruby*, *Gem*, *Git*, *Chef Client* and *Librarian Chef* (see links above). If you have installed software, its a good idea to reload open terminal windows to ensure all the new paths are added to your environment. You will also need an OpsCode account to use [Hosted Chef](http://www.opscode.com/hosted-chef/) (a free trial account should be sufficient) and access to [OpenStack](http://www.openstack.org) where you can build virtual machines.

2. Configure your Chef's knife.rb file with your keys so that it can access your Hosted Chef organisation and your Openstack account

		#An example knife.rb - update your file for your environment, organisation name and Hosted Chef keys
		current_dir = File.dirname(__FILE__)
		log_level                :info
		log_location             STDOUT
		node_name                "martynw"
		client_key               "#{current_dir}/martynw.pem"
		validation_client_name   "organisationname-validator"
		validation_key           "#{current_dir}/organisationname-validator.pem"
		chef_server_url          "https://api.opscode.com/organizations/organisationname"
		cache_type               'BasicFile'
		cache_options( :path => "#{ENV['HOME']}/.chef/checksums" )
		cookbook_path            ["#{current_dir}/../chef/cookbooks"]
		# ADD YOUR OPENSTACK KEY REFERENCES HERE
		knife[:openstack_username] = "Your OpenStack Dashboard username"
		knife[:openstack_password] = "Your OpenStack Dashboard password"
		knife[:openstack_auth_url] = "http://cloud.mycompany.com:5000/v2.0/tokens"

 Verify that your knife.rb file is correctly configured for Hosted Chef by running:

		knife cookbook list

3. Clone this Chef repository onto your computer

		mkdir ~/projects
		cd ~/projects
		git clone https://github.com/CottageLabs/dmponline-chef-repo.git dmponline-chef-repo

4. Install the cookbooks with Librarian Chef

		cd ~/projects/dmponline-chef-repo
		librarian-chef install

5. Upload the cookbooks to your Hosted Chef

		knife cookbook upload --all

6. Generate an SSH key-pair on the OpenStack dashboard and then bootstrap a new instance on OpenStack

		knife openstack server create -S KEYPAIR -N TESTNODE -f 1 -I 52 -d centos5-gems

7. Check that the instance has loaded

		knife openstack server list

*TODO: add any further steps for Openstack*
