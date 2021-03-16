# ipython

## autoreload

    %load_ext autoreload

    # Reload all modules (except those excluded by %aimport) automatically now.
    %autoreload

    # Disable automatic reloading.
    %autoreload 0

    # Reload all modules imported with %aimport every time before executing the Python code typed.
    %autoreload 1

    # Reload all modules (except those excluded by %aimport) every time before executing the Python code typed.
    %autoreload 2

    # List modules which are to be automatically imported or not to be imported.
    %aimport

    # Import module ‘foo’ and mark it to be autoreloaded for %autoreload 1
    %aimport foo

    # Mark module ‘foo’ to not be autoreloaded.
    %aimport -foo

    # Save all previous commands as python file
    %history -f /tmp/history.py

## running py files

    In [2]: cat echo_argv.py
    import sys
    print(sys.argv)

    In [3]: %run echo_argv.py 1 2 3
    ['echo_argv.py', '1', '2', '3']
