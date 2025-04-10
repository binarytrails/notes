# Vagrant

## specify vagrantfile

    VAGRANT_VAGRANTFILE=Vagrantfile.OS2 VAGRANT_DOTFILE_PATH=.vagrant_os2 vagrant up

## vbox

    vagrant plugin install vagrant-vbguest
    vagrant vbguest

centos 8: https://github.com/dotless-de/vagrant-vbguest/issues/367

## sync

    # disable default sync folder
    config.vm.synced_folder '.', '/vagrant', disabled: true

## snapshot

    vagrant snapshot save [vm-name] NAME
    vagrant snapshot restore [vm-name] NAME
    vagrant snapshot list
    vagrant snapshot delete [vm-name] NAME

## load env vars

they are loaded from ```/etc/profile.d/``` each time

    config.vm.provision "shell", inline: <<-SHELL
      echo "export MYVAR='my_value'" >> /etc/profile.d/myvar.sh
    SHELL

## ansible

### roles path

    Vagrant.configure('2') do |config|
      # manual way
      #vagrant_root = File.dirname(__FILE__)
      #ENV['ANSIBLE_ROLES_PATH'] = "#{vagrant_root}/ansible/roles:#{vagrant_root}/ansible/galaxy-roles"

      # prefered way, place custom roles in the same place (generate with ansible-galaxy init myrole --offline)
      config.vm.provision :ansible do |ansible|
        ansible.provisioning_path = '/vagrant/ansible/'
        ansible.playbook = './playbooks/play.yml'
        ansible.galaxy_role_file = 'dependencies.yml'
        ansible.galaxy_roles_path = './galaxy-roles'
      end
    end

## copy back and forth

    vagrant plugin install vagrant-scp
    vagrant scp vmname:/vagrant/file ./file

## manual config

    + set root passwd to vagrant
    + useradd -m vagrant
    - passwd -d vagrant
    + mkdir /vagrant
       chmod 775 /vagrant
       chown vagrant:vagrant /vagrant
    + visudo: vagrant ALL=(ALL) NOPASSWD: ALL
    - visudo: #Defaults requiretty
    + /etc/ssh/sshd_config:109:UseDNS no
    + vagrant ssh configuration:
       mkdir /home/vagrant/.ssh
       chmod 700 /home/vagrant/.ssh
       cd /home/vagrant/.ssh
       touch authorized_keys
       chmod 644 authorized_keys
       wget https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub --no-check-certificate
       cat vagrant.pub > authorized_keys

# vagrant plugin installation broken
```
$ vagrant plugin install vagrant-reload
Installing the 'vagrant-reload' plugin. This can take a few minutes...
Vagrant failed to properly resolve required dependencies. These
errors can commonly be caused by misconfigured plugin installations
or transient network issues. The reported error is:

conflicting dependencies json (= 2.7.2) and json (= 2.9.0)
  Activated json-2.9.0
  which does not match conflicting dependency (= 2.7.2)

  Conflicting dependency chains:
    json (= 2.9.0), 2.9.0 activated

  versus:
    json (= 2.7.2)

  Gems matching json (= 2.7.2):
    json-2.7.2

$ VAGRANT_DISABLE_STRICT_DEPENDENCY_ENFORCEMENT=1 vagrant plugin install  vagrant-reload
Installing the 'vagrant-reload' plugin. This can take a few minutes...
Fetching vagrant-reload-0.0.1.gem
Installed the plugin 'vagrant-reload (0.0.1)'!
```
