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
* Edit `credentials.yml` to set your password

```bash
# Install Ansible
pip install --requirement requirements.txt

vagrant up
vagrant provision
vagrant destroy
```

#### Vagrant Box
* https://dl.dropboxusercontent.com/u/6998388/wordpress-base/package.box
