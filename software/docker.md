# docker

    # build docker image
    docker build . -t <name> -f <Dockerfile>

    # enter bash from an image
    docker run -t -i --entrypoint="/bin/bash" <tag name or id>

    # see active containers
    docker ps

    # snapshot an active container
    docker commit <container id> <name>
