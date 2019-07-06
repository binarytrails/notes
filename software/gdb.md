# Run

    gdb --args binary --arg1 --arg2

# Threads

print all threads backtrace:

    thread apply all bt
    t a a b

# TUI Terminal UI

You can see the assembly as well as the iteractive backtrace after a ```bt```.

Jump into Terminal UI mode with ```ctrl+x+a```, type: ```up```, ```down``` or ```[enter]``` to navigate function calls.

http://www.cs.fsu.edu/~baker/ada/gnat/html/gdb_23.html

