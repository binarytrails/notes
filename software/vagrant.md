# vbox

    vagrant plugin install vagrant-vbguest
    vagrant vbguest

    # centos 8
    https://github.com/dotless-de/vagrant-vbguest/issues/367

# manual essentials

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

