# vagrant-provision-ruby

We use a really simple vagrant provisioning script for all our minimally-complex ruby projects. Instead of keeping a copy of this Vagrant provisioner in each project, we store it in teh interwebs and make a curl call when needed.

So if you have a Ruby project, and it doesn't rise to the needs of throwing Chef/Puppet/Salt/Ansible at it, this script might be sufficient. Reusing this script minimizes the amount of meta crap you have to track across your projects.

## Minimum Requirements

##### Written for Ubuntu boxes.

We currently use this one:

```
https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box
```

##### Utilizes `rbenv` and `ruby-build`

One of the best tools for managing ruby runtimes. Read more about [`rbenv`](https://github.com/rbenv/rbenv) and [`ruby-build`](https://github.com/rbenv/ruby-build).

##### Installation location must be able to make remote URL requests.

Vagrant provides `curl` by default, so you just need to make sure your network allows the external connection to GitHub. If that's not possible, grab the code and maintain it in your repo.


## Installation

### Basic, use the defaults

```ruby
Vagrant.configure("2") do |config|
  # [other config settings]
  # ...

  # Ruby dev box by forgecrafted
  # https://github.com/forgecrafted/vagrant-provision-ruby
  config.vm.provision "shell", inline: "curl -fsS https://raw.githubusercontent.com/forgecrafted/vagrant-provision-ruby/master/script | bash"
end
```

### Change the user, home directory, and/or ruby version

```ruby
# Ruby dev box by forgecrafted
# https://github.com/forgecrafted/vagrant-provision-ruby
config.vm.provision "shell", inline: <<-SCRIPT
  # Define user settings for rbenv installation.
  # Current default values (shown here) are normally fine.
  export USER_NAME=vagrant
  export USER_HOME=/home/$USER_NAME
  export DEFAULT_RUBY='2.4.2'

  curl -fsS https://raw.githubusercontent.com/forgecrafted/vagrant-provision-ruby/master/script | bash
SCRIPT
```

### Add some extra ruby installations

```ruby
# Ruby dev box by forgecrafted
# https://github.com/forgecrafted/vagrant-provision-ruby
config.vm.provision "shell", inline: <<-SCRIPT
  # Run the script first...
  curl -fsS https://raw.githubusercontent.com/forgecrafted/vagrant-provision-ruby/master/script | bash

  # Then install Ruby versions (other than $DEFAULT_RUBY) required for testing, etc.
  # The `install_ruby` syntax is REQUIRED and provided by the script
  install_ruby 1.9.3
SCRIPT
```

## Sample Vagrantfile
If the above instructions aren't making sense, or if you are new to Vagrant, [we've included a sample Vagrantfile](https://github.com/forgecrafted/vagrant-provision-ruby/blob/master/Vagrantfile-sample). It includes a call to the provisioner script, so you'll see where/how to add it to your own Vagrantfile.

We also include a set of handy config tweaks that make it into nearly all our projects.

## Bug Reports

[Drop us a line in the issues section](https://github.com/forgecrafted/vagrant-provision-ruby/issues).

**Be sure to include sample code that reproduces the problem.**

## License

Copyright (c) Forge Software, LLC. It is free software, and may be redistributed under the terms specified in the LICENSE file.
