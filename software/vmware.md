# vmware

    # connect network adapter
    systemctl start vmware-networks
    
    # boost opengl performance
    echo "mks.gl.allowBlacklistedDrivers = \"TRUE\"" >> tail .vmware/preferences
