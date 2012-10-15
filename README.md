DMPOnline "Getting Started" Chef Repository
===========================================

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

