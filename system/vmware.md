# vmware

    packer -S vmware-workstation

## post-install

    pacman -S linux-headers open-vm-tools

    modprobe -a vmw_vmci vmmon

depending on your needs:

    # enable vmtoolsd
    systemctl start vmtoolsd

    # share your network
    systemctl start vmware-networks

    # usb access
    systemctl start vmware-usbarbitrator

    # share network between vms
    systemctl start vmware-hostd
    
    # boost opengl performance
    echo "mks.gl.allowBlacklistedDrivers = \"TRUE\"" >> ~/.vmware/preferences
