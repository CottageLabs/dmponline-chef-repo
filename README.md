DMPOnline + Sword "Getting Started" Chef Repository
===========================================

Introduction
------------
This Chef repository allows you to quickly build and provision an instance of DMPOnline + Sword on a virtual server, either running locally using Virtual Box or on the cloud (e.g. via OpenStack).

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
1. First of all, make sure you have successfully installed on the prerequisite software: Ruby, Gem, Git, Chef Solo, Librarian Chef, Virtual Box and Vagrant. Its a good idea to reload open terminal windows, to ensure all the new paths are added to your environment.

2. Clone this Chef repository onto your computer

		mkdir ~/projects
		cd ~/projects
		git clone https://github.com/CottageLabs/dmponline-chef-repo.git dmponline-chef-repo

3. Install the cookbooks with Librarian Chef

		cd ~/projects/dmponline-chef-repo
		librarian-chef install

4. Review the Vagrantfile with your favourite text editor. Pay special attention to the base box used (e.g. `centos63_minimal` and its associated URL), and also the default IP address assigned to the virtual machine (e.g. `10.10.10.10`). Adjust these settings to suit your requirements and base box availability. See [www.vagrantbox.es](http://www.vagrantbox.es) for a list of base boxes. A minimal Centos 6.3 is recommended, but, with tweaking, the recipe should work with most versions of Linux.

		vi Vagrantfile

5. With the Vagrantfile configured, its time to download the base box, provision the server and install DMPOnline. This process will take some time to run, depending on your network and computer speed. Allow around 30-60 minutes to run.

		vagrant up

6. If the command completed successfully, open up a web browser and navigate to [http://10.10.10.10/](http://10.10.10.10) (or whatever you set the IP address of the virtual box to in the Vagrantfile). If everything worked, you should see the DMPOnline welcome page!

![DMPOnline screen shot](https://raw.github.com/CottageLabs/dmponline-chef-repo/master/images/dmponline.png "DMPOnline screen shot")


Using this repo
---------------

This repository uses [Librarian Chef](https://github.com/applicationsonline/librarian) to manage cookbook dependencies.

To load the cookbooks onto your Chef Server (assuming you've already set up your `knife.rb` and client keys):

	# Install librarian chef in your environment
    bundle install
    # Download the cookbook dependencies
    librarian-chef install
    # Upload them to the server
    knife cookbook upload --all

