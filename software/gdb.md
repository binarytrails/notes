# Run

    gdb --args binary --arg1 --arg2

# Threads

print all threads backtrace:

    thread apply all bt
    t a a b

# TUI Terminal UI

You can see the assembly as well as the iteractive backtrace after a ```bt```.

Jump into Terminal UI mode with ```gdb --tui``` or ```ctrl+x+a``` in a gdb.

    enter               -- enter function call
    up, down            -- scroll code
    ctrl+p, ctrl+n      -- navigate history

http://www.cs.fsu.edu/~baker/ada/gnat/html/gdb_23.html
