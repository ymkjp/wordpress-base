WordPress Base
===

[![Build Status](https://travis-ci.org/ymkjp/wordpress-base.svg?branch=master)](https://travis-ci.org/ymkjp/wordpress-base)


## Introduction

This Ansible playbook constructs WordPress, the FOSS content management system, into single server node.


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
* To get New Relic License key, generate it as following documents below
    1. [Create your New Relic account | New Relic Documentation](https://docs.newrelic.com/docs/accounts-partnerships/accounts/account-setup/create-your-new-relic-account)
    2. [License key | New Relic Documentation](https://docs.newrelic.com/docs/accounts-partnerships/accounts/account-setup/license-key)

* Run command below, and edit `credentials.yml` to set your passwords and API Keys

```bash
$ cat <<EOF > credentials.yml
admin_password:       "$(openssl passwd -salt foo -1 bar)"  # password: "bar"
wp_db_password:       "$(openssl rand -hex 16)"
mysql_root_password:  "$(openssl rand -hex 16)"
newrelic_api_key:     "__YOUR_API_KEY__"
EOF
```

* Install Ansible and its 3rd party packages to your local client

```bash
$ pip install --requirement requirements.txt
$ ansible-galaxy install --role-file=role_packages.yml
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

* Run commands below, and wait for a few hours or so (up to the server spec)
    * Use `--check` option if you prefer to run as dry-run mode

```bash
$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@example.com
$ ansible-playbook site.yml --inventory-file hosts --extra-vars "@credentials.yml" --private-key="~/.ssh/id_rsa" --limit="centos6_init"
$ ansible-playbook site.yml --inventory-file hosts --extra-vars "@credentials.yml" --private-key="~/.ssh/id_rsa" --limit="wordpress_server"
```

#### How to know what are these commands above doing?

```bash
$ ansible-playbook site.yml --list-tasks
$ ansible-playbook site.yml --list-hosts
```
