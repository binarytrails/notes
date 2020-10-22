# tmux

## keyboard

    ctrl+b  sends <command> to tmux instead of sending it to the shell
    
    ?	    shows a list of all commands (qcloses the list)
    :	    enter a tmux command
    
    c	    creates a new window
    ,	    rename current window
    p	    switch to previous window
    n	    switch to next window
    w	    list windows (and then select with arrow keys)
    
    %	    split window vertically
    -	    split window horizontally
    →	    go to right pane
    ←	    go to left pane
    ↑	    go to upper pane
    ↓	    go to lower pane
    
    d	    detach from session

## command line

    tmux -s	            creates a new session with name <SESSION NAME>
    tmux list-sessions	lists all currently running tmux sessions
    tmux attach -t	    attach to the session called <SESSION NAME>

### flow sample

    # start a new tmux session and detach from it
    tmux new-session -d -s session1

    tmux rename-window 'my window'
    tmux send-keys 'echo "pane 1"' C-m

    tmux select-window -t session1:0
    tmux split-window -h
    tmux send-keys 'echo "pane 2"' C-m

    tmux split-window -h
    tmux send-keys 'echo "pane 3"' C-m

    # we want to have notifications in the status bar, if there are changes in the windows
    tmux setw -g monitor-activity on
    tmux set -g visual-activity on

    tmux select-layout even-horizontal

    # select the first window to be in the foreground
    tmux select-window -t session:1

    # attach our terminal to the tmux session
    tmux -2 attach-session -t cflogs


## config

    # ~/.tmux.conf
    set -g mode-mouse on
    set -g mouse-resize-pane on
    set -g mouse-select-pane on
    set -g mouse-select-window on
