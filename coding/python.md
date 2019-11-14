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