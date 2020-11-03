# check if command exists

    #!/usr/bin/env bash

    command_name="tototata"
    command=`which $command_name`

    if [ -z "$command" ]; then
        echo "Command not found"
    else
        echo "$command"
    fi
