# virtualenv

    # build-in in python3
    python -m venv ENV

# attributes

    args.foo                        # raises an AttributeError

    hasattr(args, 'foo')            # returns False
    getattr(args, 'foo', 'other')   # returns 'other'

    sys.sizeof([1,2])               # includes marginal space (garbage collection overhead)
    [1,1].__sizeof__()              # space allocation of an object w/o additional garbage value

    > obj.__dict__                  # get class attributes by name
    {'b': 1, 'c': 2}

    > dir(obj)                      # get callable names
    ['__file__', '__name__']

# arithmetics

    2**2 == math.pow(2, 2)                  # powers
    bin(int("001",2) + int("001",2))[2:]    # binary addition

    > set([1,2,3]) ^ set([3, 1])            # symmetric difference: return a new set with elements
                                            # in either the set or other but not both
    {2}

# generator expression

    >>> dicts = [
        { "name": "Tom", "age": 10 },
        { "name": "Mark", "age": 5 },
        { "name": "Pam", "age": 7 },
        { "name": "Dick", "age": 12 }
    ]
    >>> next(item for item in dicts if item["name"] == "Pam")
    {'age': 7, 'name': 'Pam'}

# parsing

## text files

    In [3]: from string import Template
       ...: 
       ...: d = {
       ...:     'title': 'This is the title',
       ...:     'subtitle': 'And this is the subtitle',
       ...:     'list': '\n'.join(['first', 'second', 'third'])
       ...: }
       ...: template = """$title
       ...: $subtitle
       ...: $list
       ...: """
       ...: src = Template(template)
       ...: result = src.substitute(d)
       ...: print(result)
       ...: 
    This is the title
    And this is the subtitle
    first
    second
    third

# pip

    # pip anywhere
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py
    python -m pip install -U whatyouneed=1.0.0

    # install pip packages as normal user
    pip install <package> --user
    pip list -v

    # record setup.py installed files in list
    python setup.py install --record files.txt
    cat files.txt | xargs rm -rf

# flask

    # smol server
    import flask
    app = flask.Flask('smol-server')
    @app.route('/', methods=['POST'])
    def root():
        print(flask.request.json)
        return flask.Response('success')
    app.run(host='127.0.0.1', port=8080, debug=True)

    # generate cookie
    response = flask.Response('Hello World') # can be html
    response.set_cookie('mycookie', cookie, domain='www.mydomain.com')

    # load cookie
    cookie = request.cookies.get('mycookie')

# process

## detach process

    import subprocess

    subprocess.Popen(
        ["watch", "ls"], bufsize=-1, executable=None,
        stdin=None, stdout=None, stderr=None,
        close_fds=True, shell=True, cwd=None, env=None
    )
  
## system command as main process
    
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
