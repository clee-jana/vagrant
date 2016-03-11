# Disclaimer

The work in this repo is very much an in-progress proof-of-concept. Expect it to change a lot without warning. The goal is to make development environment as hassle free as possible, with a minimum amount of setup time. As we increase the number of services in our environment, development will become more cumbersome. This project aims to address some of those issues. If you are interested, join the #vagrant slack channel!

## Current Goal

Get a apollo development environment working. We're starting with Apollo because it does not have a front end.

* ~~Be able to pip install~~
* Connect To Cassandra and Redis
* Run Unit Tests and Integration Tests

## Lessons Learned

* I initially struggled with my first crack at creating the virtualenv inside the apollo directory with an error `ImportError: cannot import name timedelta`. After investigation, it's because the underlying OS X filesystem is not case sensitive, and modules in Common that attempt to import datetime end up picking up Common.DateTime instead of the python datetime. The workaround was to create the virtualenv outside of the synced folder, and I decided to add virtualenvwrapper to the dependencylist. This probably works out better anyway because you won't get the shared venv confused with your host system.


## Installation Instructions

* Download [Vagrant](http://vagrantup.com)
* Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Check out this repo to the place you have your other Jana repos. The path in the Vagrantfile expects to be sitting at the same level as your other projects.

For Example:

```
+ /home/clee/projects
  + apollo
  + engineering
  + vagrant (this repo)
```

## Getting Started

### SSH Forward Agent

The Jana requirements files have dependencies that require downloading
repos from GitHub. We'll use the `ssh.forward_agent` configuration
option in the Vagrantfile, but in your host machine you'll still have to
use `ssh-add` to make your key available to the vagrant machine. For
simple setups, just issuing the command will add your ssh key.

```
$> ssh-add
Identity added: /Users/clee/.ssh/id_rsa (/Users/clee/.ssh/id_rsa)
```

### Starting Vagrant

```
# from this repos home
$> vagrant up
# to resume a suspended instance
$> vagrant resume
# ssh into the isntance
$> vagrant ssh

# inside the vagrant image now
vagrant $> export JANAENV=develop
vagrant $> mkvirtualenv apollo
vagrant $> workon apollo
vagrant $> cd /usr/local/jana/repo/apollo/current
vagrant $> pip install -r requirements.txt -r test_requirements.txt
```

## Stopping the instance
```
# standby
$> vagrant suspend

# shutdown
$> vagrant halt

# delete the instance to start over. please note you will lose the contents of the vagrant box. synced folders are safe because they are on the host vm.
$> vagrant destroy
