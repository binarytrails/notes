# Run

    gdb --args binary --arg1 --arg2

# Breakpoints

start from second breakpoint

scenario:

    void bp2() { };
    void bp1() { bp2(); }

    int main()
    {
      bp2();
      bp1();
      return 0;
    }

gdb, trigger when bp1->bp2:

    b bp1
    b bp2

    # set commands to be executed when the given breakpoints are hit.
    # give a space-separated breakpoint list as argument after "commands".
    commands 1
    silent
    enable 2
    c
    end

    commands 2
    disable 2
    end

    disable 2

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

# Core dump

    coredumpctl list
    ulimit -c unlimited
    ulimit -a | grep core

    # alternatively
    gdb binary core
