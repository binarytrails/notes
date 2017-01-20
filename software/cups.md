# Arch Linux

    sudo pacman -S gutenprint   # enormous drivers repository (cups in deps)
  
    sudo gpasswd -a <user> lp
    sudo gpasswd -a <user> sys
    sudo gpasswd -a <user> scanner
  
    sudo systemctl start org.cups.cupsd.service
    sudo systemctl enable org.cups.cupsd.service
    
    firefox http://127.0.0.1:631/printers/
    # select add a printer and enter your <user> : <passwd>

    # now you should be able to print from any software
