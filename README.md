posty Cookbook
===========================================================
Chef cookbook for a full posty mail server installation with
[Postfix](http://www.postfix.org/),
[Dovecot](http://www.dovecot.org/),
[Roundcube](http://roundcube.net/),
[ClamAV](http://clamav.net/),
[SpamAssassin](http://spamassassin.apache.org/) and
[Posty](http://www.posty-soft.org/).


Requirements
------------

### Cookbooks
The following external cookbooks are used:

* [apt](https://github.com/opscode-cookbooks/apt)
* [clamav](https://github.com/RoboticCheese/clamav-chef)
* [mysql](https://github.com/opscode-cookbooks/mysql)
* [ruby_build](https://github.com/fnichol/chef-ruby_build)
* [cron](https://github.com/opscode-cookbooks/cron) (via clamav)
* [logrotate](https://github.com/stevendanna/logrotate) (via clamav)

Additionally the following cookbooks must be present due to dependencies, but aren't executed:

* [yum](https://github.com/opscode-cookbooks/yum)
* [yum-epel](https://github.com/opscode-cookbooks/yum-epel)
* [yum-mysql-community](https://github.com/opscode-cookbooks/yum-mysql-community)

All dependencies are automatically resolved when using Berkshelf

### Platform
The following platforms are currently supported and tested:

* Ubuntu 12.04
* Ubuntu 14.04
* Debian 7.4


Configuration
-------------
Before starting the installation of the posty system it is recommended that you
configure your own passwords (and other settings if you wish) in
`config/posty.json`


Usage
-----
This recipe can be used in multiple ways. The Vagrant method is recommended for
inexperienced Chef users.

**Warning**: This recipe installs a complete mail server with many individual
programms, thus changes a lot of system files. Using it on an unconfigured
system is recommended.

This recipe's original purpose is to provide an easy way to try out the
posty-api. For usage in a production environment a manual customization/ review
of the configuration is recommended.

#### Chef Server
Just include `posty` in your node's `run_list`:

```json
{
  "name":"my_node",
  "run_list": [
    "recipe[posty]"
  ]
}
```

#### Vagrant
For the development of this cookbook and to allow easy testing of the posty
system, an automated process to set up a virtual test/development machine is
provided.

##### Requirements
* [VirtualBox](https://www.virtualbox.org)
* [Vagrant](http://vagrantup.com) 1.6+
* [Vagrant Omnibus](https://github.com/schisamo/vagrant-omnibus)
* [Vagrant Berkshelf](https://github.com/berkshelf/vagrant-berkshelf) 2.0.1+

##### Vagrant installation with Ubuntu as host
* Download the latest Vagrant version from the [project page](http://www.vagrantup.com/downloads.html)
* `dpkg -i vagrant*.deb; apt-get install -f` or install the deb via Ubuntu Software Center
* `vagrant plugin install vagrant-omnibus` to install Omnibus
* `vagrant plugin install vagrant-berkshelf --plugin-version '>= 2.0.1'` to install Berkshelf

##### Bootstrapping the Virtual Development Machine

```
git clone https://github.com/posty/posty_chef
cd posty_chef
vagrant up
```

This sets up a virtual machine host __posty-chef__ based on Ubuntu 14.04
providing all services mentioned above.

The IP address assigned to the host is 192.168.254.10 which can be changed
by adapting the parameter "config.vm.network" in the Vagrantfile accordingly.

The setup takes a couple of minutes. After the installation has finished
you can login to the machine by running: `vagrant ssh`
The posty_api and the roundcube webmailer are reachable via HTTP and HTTPS
under the configured [IP](http://192.168.254.10). The mail services are
reachable under the same IP.


License and Authors
-------------------
See LICENSE file.
