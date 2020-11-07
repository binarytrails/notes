# check if command exists

    #!/usr/bin/env bash

    command_name="tototata"
    command=`which $command_name`

    if [ -z "$command" ]; then
        echo "Command not found"
    else
        echo "$command"
    fi

# loop over files with spaces

    find . -type f -name "*.priv" -print0 | while IFS= read -r -d '' file; do echo "$file"; done

# get filename no extension

    "${pkey%.*}"
