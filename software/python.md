# virtualenv

    # build-in in python3
    python -m venv ENV

# attributes

    args.foo                        # raises an AttributeError
    hasattr(args, 'foo')            # returns False
    getattr(args, 'foo', 'other')   # returns 'other'

# detach process

    import subprocess

    subprocess.Popen(
        ["watch", "ls"], bufsize=-1, executable=None,
        stdin=None, stdout=None, stderr=None,
        close_fds=True, shell=True, cwd=None, env=None
    )
