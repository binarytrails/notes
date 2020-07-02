    # build in virtualenv in python3
    python -m venv ENV

    # attributes
    args.foo                        # raises an AttributeError
    hasattr(args, 'foo')            # returns False
    getattr(args, 'foo', 'other')   # returns 'other'
