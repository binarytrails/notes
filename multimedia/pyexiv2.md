# In virtualbox

    virtualenv -p python2.7 ENV

## Arch

    sudo pacman -S python2-exiv2
    cd ENV/lib/python2.7/ && ln -s /usr/lib/python2.7/site-packages/pyexiv2/ && ln -s /usr/lib/python2.7/site-packages/libexiv2python.so 

## Debian

    sudo apt-get install pyexiv2
    cd ENV/lib/python2.7/ && ln -s /usr/lib/python2.7/dist-packages/pyexiv2/ && ln -s /usr/lib/python2.7/dist-packages/libexiv2python.so

