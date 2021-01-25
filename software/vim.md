# Colon commands

    :Ex                 Browse file directory & show what you edited but didn't save
    :cq                 Quit with an error code (abort git commit --amend)
    :'[id]              Jump among recently edited files located in ~/.viminfo
    :browse old         Get a menu with the Recently edited files numbers
    :retab              Replace tabs according to your settings
    :e ++ff=unix        Display CRLF as ^M
    :%s/\r//g           Remove ^M
    :%s/\s\+$//g        Remove trailing white spaces
    :%s/^\n//g          Remove empty white lines
    :w !sudo tee %      Save as sudo

    :set background=light
    :python import sys; print(sys.version)      Get the compiled python version
    :au BufWritePost * make                     Build with make when saving a file :w

# Keyboard shortcuts

    $   go to the end of the line
    A   go to the end of the line and switch to editing mode
    0   go to the beginning of the line incl. whitespace
    ^   go to the first non-whitespace character in the line
    U   make uppercase
    u   make lowercase

    q:  list commands history
    q/  list searches history
    
# Keys

    ctrl + v, tab       insert tab in insert mode

# Show

    /\t                 show all interpreted tabs (if spaces set then spaces)
    /\s\+$              trailing whitespaces
    /\S\zs\s\+$         trailing whitespaces + ignore empty lines
    / \+\ze\t           spaces before tabs

# Vim clipboard

> You do need to install the gvim package to get a Vim linked to libX11; but you don't need to use gvim. --Carpetsmoker

Copy paths to clipboard

    " relative path
    :let @+ = expand("%")

    " full path
    :let @+ = expand("%:p")

    " just filename
    :let @+ = expand("%:t")

