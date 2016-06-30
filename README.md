# Chef Cookbook Wrapper + Wordpress

Description:
===========
This Chef Cookbook Wrapper is a custom cookbook based on brint/wordpress-cookbook.

The Chef WordPress cookbook installs and configures WordPress according to the instructions at http://codex.wordpress.org/Installing_WordPress.

Usage:
=====
Clone this repo and follow the steps below :

1. zzz
2. ddd 


Tutorial: Create your custom cookbook wrapper from scratch
=====
1. Follow **first Chef tutorial to create your environment**: [https://learn.chef.io/learn-the-basics/ubuntu/configure-a-resource/]() Create the chef-repo directory under your home directory, `~/` 
	
	```shell
	mkdir ~/learn-chef
	cd ~/learn-chef
	```
	
2. **Generate** your custom cookbook
	
	```shell
	chef generate cookbook chef-cookbook-wrapper-wordpress 
	```
3. Edit the file `metadata.rb` to add **dependencies**
	
	```ruby
	depends 'apt', '~> 2.9.2'
	depends 'wordpress', '~> 3.0.0' 
	```
4. Edit `recipes/default.rb` to **include Wordpress default recipe** from Chef Supermarket

	```ruby
	include_recipe 'wordpress::default'
	```

5. Edit `.kitchen.yml` to configure your VM with your custom `driver`, `IP address`, `OS`... Set **your custom private network ip address.**

	```yml
	---
	driver:
	  name: vagrant
	  network:
	    - ["private_network", { ip: "192.168.36.37" }]
	
	provisioner:
	  name: chef_zero
	
	platforms:
	  - name: centos-7.1
	
	suites:
	  - name: default
	    run_list:
	      - recipe[chef-cookbook-wrapper-wordpress::default]
	    attributes:
	```


5. Edit `attributes/default.rb` to override Wordpress server aliases with **your custom virtual host**

	```ruby
	default['wordpress']['server_aliases'] = ['example.local']
	```
	
6. Edit your `hosts`file to **add your custom vhost with your ip address and custom virtual host** above :

	- **Mac OS**: run this command to edit the file `open /private/etc/hosts`

	- **Ubuntu**: run this command to edit the file `vi /etc/hosts`

	```shell
	# enter your custom ip address and virtual host
	192.168.36.37 	example.local
	```	
	
7. Run this latest command to **run your cookbook**

	```shell
	cd ~/learn-chef/chef-cookbook-wrapper-wordpress
	kitchen converge
	
	...
	
	# this command is finished when you get the following message
	#-----> Kitchen is finished. (Xm Xs) 
	```	

8. If necessary use this command to destroy cookbook **if you have made changes to the source code**

	```shell
	kitchen destroy
	```	

9. Check if everything is good with **ping** command

	```shell
	ping example.local
	PING example.local (192.168.36.37): 56 data bytes
	```	

10. Open `http://example.local/wp-admin/install.php` in your browser to **install your wordpress site like usual**.
