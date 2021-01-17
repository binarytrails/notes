# pip anywhere

    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py
    python -m pip install -U whatyouneed=1.0.0

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

# Arithmetics

    2**2 == math.pow(2, 2)                      # powers
    bin(int("001",2) + int("001",2))[2:]        # binary addition

# Install pip packages as normal user

Append --user at the end to install to ~/.local/lib/python2.7/site-packages

Verify their locations with:

    pip list -v

# Record setup.py installed files in list

    python setup.py install --record files.txt

Once you want to uninstall you can use xargs to do the removal:

    cat files.txt | xargs rm -rf

# Get class attributes by name

    > class A(object):
        def __init__(self):
            self.b = 1
            self.c = 2
     
    > a = A()
    
    > a.__dict__
    {'b': 1, 'c': 2}
    
    > a.__dict__.get('b')
    1

# Get Callable Names
https://docs.python.org/2/library/functions.html#dir

    import struct
     
    > dir() # show the names in the module namespace
    ['__builtins__', '__doc__', '__name__', 'struct']
     
    > dir(struct) # show the names in the struct module
    ['Struct', '__builtins__', '__doc__', '__file__', '__name__',
    '__package__', '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
    'unpack', 'unpack_from']
     
    > class Shape(object):
        def __dir__(self):
            return ['area', 'perimeter', 'location']
        
    > s = Shape()
    
    > dir(s)
    ['area', 'perimeter', 'location']
  
 # System command as main process
    
    command = "foo arg1 arg2"
    try:
        loud_proc = subprocess.Popen(command,
            shell = True,
            stdout = subprocess.PIPE,
            stderr = subprocess.STDOUT
        )
        
        # The child return code as None value indicates that the process hasn’t terminated yet.
        while loud_proc.poll() is None:
            print loud_proc.stdout.readline()
        
    except KeyboardInterrupt:
        print "Got Keyboard interrupt."

# Do not define PYTHONPATH

Avoid placing Python-specific configuration into your shell env directly.
Specify Python paths, variables, etc inside the Python scripts.
It allows each Python script or program to call Python its own way.

In Debian Jessie, 

    export PYTHONPATH=/usr/lib/python2.7/
    
Will make crash the python-virtualenv 1.11.6+ds-1 package.
