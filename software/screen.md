# screen

## inside controls

    ctrl+a d        detach
    ctrl+a c        create new window with shell
    ctrl+a A        rename current window
    ctrl+a "        list all window
    ctrl+a 0        switch to window 0
    ctrl+a S        split current region horizontally into two regions
    ctrl+a |        split current region vertically into two regions
    ctrl+a tab      switch the input focus to the next region
    ctrl+a Ctrl+a   toggle between the current and previous region
    ctrl+a Q        close all regions but the current one
    ctrl+a X        close the current region

## outside controls

**reattach to a detached**

    screen -d -r <name>

### kill detached sessions

    screen -ls | grep Detached | cut -d. -f1 | awk '{print $1}' | xargs kill
