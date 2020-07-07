# pass

    pass init <gpg-key-id>

## insert pass

    pass insert dir/name

## generate pass

    pass generate dir/name <length>

## show dirs with passwords

    pass show

## copy to clipboard

keeps by default for 45 seconds

    pass -c dir/name

## delete a key

    pass rm dir/key

## delete directory

    pass rm -r dir

## backup

it will copy all dirs with pass.gpg files:

    cp -r ~/.password-store <destination>
