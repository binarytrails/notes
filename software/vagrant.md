# Vagrant

## specify vagrantfile

    VAGRANT_VAGRANTFILE=Vagrant-2 vagrant up

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
