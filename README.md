# Chef Cookbook Wrapper + Wordpress

![alt text](https://www.chef.io/blog/wp-content/uploads/2014/12/chefiomove-1024x514.png "Chef cookbook wrapper + Wordpress")

Description:
===========
This Chef Cookbook Wrapper is a custom cookbook based on brint/wordpress-cookbook.

The Chef WordPress cookbook installs and configures WordPress according to the instructions at http://codex.wordpress.org/Installing_WordPress.

Usage:
=====

- To run this cookbook _quickly_, see point **7 to 10 below** to configure your virtual host on your workstation and run cookbook. Your default ip address is   `192.168.36.37`. Your default virtual host is `example.local`. You can change default values to following Tutorial below (steps 5/6/7/8). 
- To _create your custom cookbook from scratch_ see **Tutorial** section below


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

6. Generate **`attributes`** folder

	```ruby
	chef generate attribute default
	```

7. Edit `attributes/default.rb` to override Wordpress server aliases with **your custom virtual host**

	```ruby
	default['wordpress']['server_aliases'] = ['example.local']
	```
	
8. Edit your `hosts`file to **add your custom vhost with your ip address and custom virtual host** above :

	- **Mac OS**: run this command to edit the file `open /private/etc/hosts`

	- **Ubuntu**: run this command to edit the file `vi /etc/hosts`

	```shell
	# enter your custom ip address and virtual host
	192.168.36.37 	example.local
	```	
	
9. Run this latest command to **run your cookbook**

	```shell
	cd ~/learn-chef/chef-cookbook-wrapper-wordpress
	kitchen converge
	
	...
	
	# this command is finished when you get the following message
	#-----> Kitchen is finished. (Xm Xs) 
	```	

10. If necessary use this command to destroy cookbook **if you have made changes to the source code**

	```shell
	kitchen destroy
	```	

11. Check if everything is good with **ping** command

	```shell
	ping example.local
	PING example.local (192.168.36.37): 56 data bytes
	```	

12. Open `http://example.local/wp-admin/install.php` in your browser to **install your wordpress site like usual**.
