# Concourse CI Demo

[Concourse CI](http://concourse.ci) is a pipeline-based CI system written in Go. This demo uses the [Concourse-Lite](http://concourse.ci/vagrant.html) installation (based on [Vagrant](https://www.vagrantup.com)).

## Pre-Requisite

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Install [Vagrant](https://www.vagrantup.com).
3. Check out this Git repository:

	```bash
	git clone https://github.com/pivotal-sg/concourse-demo.git
	```

## Setting up Concourse CI - Lite

1. Start Concourse-Lite

	```bash
cd concourse-demo
vagrant up
```

	And wait for the vagrant box to download and start up. When it is done, you should see this:
	
	```bash
	Bringing machine 'default' up with 'virtualbox' provider...
	==> default: Importing base box 'concourse/lite'...
	==> default: Matching MAC address for NAT networking...
	==> default: Checking if box 'concourse/lite' is up to date...
	==> default: Setting the name of the VM: concourse-demo_default_1468288318601_62684
	==> default: Clearing any previously set network interfaces...
	==> default: Preparing network interfaces based on configuration...
	    default: Adapter 1: nat
	    default: Adapter 2: hostonly
	==> default: Forwarding ports...
	    default: 22 => 2222 (adapter 1)
	==> default: Running 'pre-boot' VM customizations...
	==> default: Booting VM...
	==> default: Waiting for machine to boot. This may take a few minutes...
	==> default: Machine booted and ready!
	==> default: Checking for guest additions in VM...
	==> default: Setting hostname...
	==> default: Configuring and enabling network interfaces...
	==> default: Mounting shared folders...
	    default: /vagrant => /Users/neo/workspace/concourse-demo
	==> default: Running provisioner: shell...
	    default: Running: inline script
	    ...
	==> default: Setting up liberror-perl (0.17-1.1) ...
	==> default: Setting up git-man (1:1.9.1-1ubuntu0.3) ...
	==> default: Setting up git (1:1.9.1-1ubuntu0.3) ...
	==> default: Setting up git-core (1:1.9.1-1ubuntu0.3) ...
	```

2. Make your own copy of `credentials.yml`

	```bash
	cp credentials-sample.yml credentials.yml
	```

3. Add `vagrant` user’s SSH key for Rsync
	* Get the location of the SSH key used by Vagrant: 

		```bash
		vagrant ssh-config
		```	
		Example.

		```bash
		➜  concourse-demo git:(master) ✗ vagrant ssh-config
		Host default
		  HostName 127.0.0.1
		  User vagrant
		  Port 2222
		  UserKnownHostsFile /dev/null
		  StrictHostKeyChecking no
		  PasswordAuthentication no
		  IdentityFile /Users/pivotal/workspace/concourse-demo/.vagrant/machines/default/virtualbox/private_key
		  IdentitiesOnly yes
		  LogLevel FATAL
		```
	* Get the contents of the `private_key` and paste it into the `rsync-private-key` attribute of `credentials.yml`.

		```bash
		cat /Users/pivotal/workspace/concourse-demo/.vagrant/machines/default/virtualbox/private_key
		```

4. Visit [http://192.168.100.4:8080](http://192.168.100.4:8080) and download the [Fly CLI](http://concourse.ci/fly-cli.html).

	![Default screen](http://concourse.ci/hello-cli-downloads.png)
	
	** ***Move the downloaded file onto your PATH***

	```bash
	mv fly /usr/local/bin
	```

## Building a sample Rails app

1. Add new pipeline:

	```bash
	fly -t lite set-pipeline -p spotlight -c spotlight.yml --load-vars-from credentials.yml
	```

2. Unpause the new pipeline:

	```bash
	fly -t lite unpause-pipeline -p spotlight
	```

3. Visit [http://192.168.100.4:8080](http://192.168.100.4:8080) to see the build in progress.

4. You may view the list of builds:

	```bash
	fly -t lite builds
	```

5. You can also log in to one of the build container instances:

	```bash
	fly -t lite hijack -j spotlight/spotlight
	```

## Other Resources

* [Hello, world!](https://concourse.ci/hello-world.html)
* [Official Tutorials](https://concourse.ci/tutorials.html)
* [Start and Wayne Concourse Tutorial](https://github.com/starkandwayne/concourse-tutorial)