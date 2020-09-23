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

# generator expression

    >>> dicts = [
        { "name": "Tom", "age": 10 },
        { "name": "Mark", "age": 5 },
        { "name": "Pam", "age": 7 },
        { "name": "Dick", "age": 12 }
    ]
    >>> next(item for item in dicts if item["name"] == "Pam")
    {'age': 7, 'name': 'Pam'}

# flask

    # generate cookie
    response = flask.Response('Hello World') # can be html
    response.set_cookie('mycookie', cookie, domain='www.mydomain.com')

    # load cookie
    cookie = request.cookies.get('mycookie')

# ipython

## autoreload

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
