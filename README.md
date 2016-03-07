## WordPress

#### Requirement
- Python v2.7.10
- Vagrant v1.8.0

```bash
# Example on OS X with Homebrew
xcode-select --install

brew update && brew install \
  Caskroom/cask/vagrant \
  python
```

#### Setup
* Edit `credentials.yml` to set your passwords and API Keys

```ruby
wp_db_password:       "__YOUR_PASSWORD__"
mysql_root_password:  "__YOUR_PASSWORD__"
newrelic_api_key:     "__YOUR_API_KEY__"
newrelic_license_key: "__YOUR_LICENSE_KEY__"
```

* Install Ansible, and run Vagrant commands

```bash
# Install Ansible
pip install --requirement requirements.txt

vagrant up
vagrant provision
vagrant destroy
```

#### Vagrant Box
* https://dl.dropboxusercontent.com/u/6998388/wordpress-base/package.box
