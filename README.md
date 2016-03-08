## WordPress Base

#### Environment
* Ansible Host: CentOS 6.7
* Ansible Client: Mac OS X 10.11

#### Requirement
- Python v2.7
- Vagrant v1.8 (optional)

```bash
# Example on OS X with Homebrew
xcode-select --install \
&& brew update \
&& brew install \
  python \
  Caskroom/cask/vagrant
```

#### Setup Client
1. Edit `credentials.yml` to set your passwords and API Keys
    * Run `openssl passwd -salt foo -1 bar` to generate `admin_password`

```ruby
admin_password:       "__YOUR_PASSWORD__"
wp_db_password:       "__YOUR_PASSWORD__"
mysql_root_password:  "__YOUR_PASSWORD__"
newrelic_api_key:     "__YOUR_API_KEY__"
newrelic_license_key: "__YOUR_LICENSE_KEY__"
```

2. Install Ansible to your local client

```bash
# Install Ansible and its 3rd party packages
$ pip install --requirement requirements.txt
```

3. Edit ``~/.ssh/config`` corresponding to hosts (No need for Vagrant)

```bash
$ cat <<EOF >> ~/.ssh/config
Host centos6-init001
    User root
    HostName tk2-001-00001.vs.sakura.ne.jp
    IdentitiesOnly yes

Host centos6-general001
    User admin
    HostName tk2-001-00001.vs.sakura.ne.jp
EOF
```

#### Setup Vagrant

* Run Vagrant commands after setting up
* See Vagrant Box if you prefer
  * [wordpress-base/package.box](https://dl.dropboxusercontent.com/u/6998388/wordpress-base/package.box)

```bash
$ vagrant login  # Sign up to Hashicorp before logging in
$ vagrant up  # Run this for the first time
$ vagrant provision  # Run this when `vagrant up` had problem due to network error, etc
$ vagrant destroy --force && vagrant up  # Run this to reset everything
```

#### Setup CentOS 6.7

* Run commands below

```
$ ansible-playbook -i hosts centos6_init.yml -k
$ ansible-playbook -i hosts site.yml -k
```

#### Tips

```bash
$ $ TARGET_TASK_NAME="Install Mysql packages" \
    ansible-playbook --start-at "${TASK_NAME}" \
    --extra-vars "@credentials.yml" site.yml
```
