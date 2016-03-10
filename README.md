WordPress Base
===

[![Build Status](https://travis-ci.org/ymkjp/wordpress-base.svg?branch=master)](https://travis-ci.org/ymkjp/wordpress-base)


## Introduction

This Ansible playbook constructs WordPress, the FOSS content hosing server, into single node.


## Requirements

#### Environment
* Host: CentOS 6.7
* Client: Mac OS X 10.11

#### Runtime
- Python v2.7
- Vagrant v1.8 (optional)
- ssh-copy-id (optional)

```bash
# Example on OS X with Homebrew
xcode-select --install \
&& brew update \
&& brew install \
  python \
  ssh-copy-id \
  Caskroom/cask/vagrant
```

## Setups

#### Setup Client
* Edit `example.credentials.yml` to set your passwords and API Keys
    * Run `openssl passwd -salt foo -1 bar` to generate `admin_password`
* Rename the file, such as by `mv example.credentials.yml credentials.yml`

```yml
admin_password:       "__YOUR_PASSWORD__"
wp_db_password:       "__YOUR_PASSWORD__"
mysql_root_password:  "__YOUR_PASSWORD__"
newrelic_api_key:     "__YOUR_API_KEY__"
```

* Install Ansible to your local client

```bash
# Install Ansible and its 3rd party packages
$ pip install --requirement requirements.txt
```

* Edit ``~/.ssh/config`` corresponding to hosts (No need for Vagrant)

```bash
$ cat <<EOF >> ~/.ssh/config
Host centos6-init001
    User root
    HostName example.com
    IdentitiesOnly yes

Host centos6-general001
    User admin
    HostName example.com
EOF
```

#### Setup Vagrant Server for local development environment

* Run Vagrant commands after setting up client

```bash
$ vagrant login  # Sign up to Hashicorp before logging in
$ vagrant up  # Run this for the first time
$ vagrant provision  # Run this when `vagrant up` had problem due to network error, etc
$ vagrant destroy --force && vagrant up  # Run this to reset everything
```

#### Setup CentOS 6 Host Server for production environment

* Run commands below

```
$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@example.com
$ ansible-playbook --inventory-file hosts --extra-vars "@credentials.yml" --private-key="~/.ssh/id_rsa" site.yml
```

#### Tips

```bash
$ ansible-playbook --start-at='Set up iptables rules' --extra-vars "@credentials.yml" site.yml
```
