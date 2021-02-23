# Issues
## Arch Linux
### Missing libraries

There is two ways to arrange the original openshot-1.4.3 package.

1. Downgrade to the mlt to AUR package

Error:

    Failed to import 'from openshot import main'
    Error Message: cannot import name main

Solution:

    Change the library mlt 0.9.6-7 (extra) -> mlt-git v0.9.2.r247.ge06718f-1 (aur).
    
    Use aura to include all dependecies and automate the tasks:
    >> sudo aura -A mlt-git


2. Keet the mlt 0.9.6-7 & get the missings libraries

Error:

    mlt_repository_init: failed to dlopen /usr/lib/mlt/libmltvidstab.so
      (libvidstab.so.0.9: cannot open shared object file: No such file or directory)
    mlt_repository_init: failed to dlopen /usr/lib/mlt/libmltsdl.so
      (libSDL_image-1.2.so.0: cannot open shared object file: No such file or directory)
    mlt_repository_init: failed to dlopen /usr/lib/mlt/libmltsox.so
      (libsox.so.3: cannot open shared object file: No such file or directory)
    mlt_repository_init: failed to dlopen /usr/lib/mlt/libmltqt.so
      (libQt5Svg.so.5: cannot open shared object file: No such file or directory)

Solution:
Keep the 0.9.6-7 version and install all the optional dependecies.
    
    >> sudo pacman -S vid.stab sdl_image sox qt5-svg

Error:

    ------------------------- ERROR 1 ------------------------------
    Failed to import 'from openshot import main'
    Error Message: No module named xdg.IconTheme
    ----------------------------------------------------------------
    
    ------------------------- ERROR 2 ------------------------------
    Failed to import 'from openshot import main'
    Error Message: No module named goocanvas
    ----------------------------------------------------------------

Solution:

    sudo pacman -S python2-xdg goocanvas pygoocanvas

Optional Dependencies:

To avoid further library related errors because openshot has many usefull features:

    sudo pacman -S libsamplerate libdv frei0r-plugins libexif ffmpeg-compat

Finally, if there is still something wrong:

    sudo pacman -S libftdi-compat  libirman  libusb-compat  libxss  libxxf86dga  lirc mplayer  python2-httplib2  scrnsaverproto  xf86dgaproto

