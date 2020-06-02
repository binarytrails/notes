# docker

## command line

    # build docker image
    docker build . -t <name> -f <Dockerfile>

    # enter bash from an image
    docker run -t -i --entrypoint="/bin/bash" <tag name or id>

    # execute in a container
    docker exec -ti <name> ls -l

    # synch folder
    docker cp <local path> <docker path>

    # see active containers
    docker ps

    # snapshot an active container
    docker commit <container id> <name>

    # cleanup everything
    docker rm $(docker ps -a -q) && docker rmi $(docker images -q)

## Dockerfile

    # Sync folder
    WORKDIR <docker path>
    COPY <local path> <docker path>

    # Pass to entrypoint on docker run (you pass arguments to it with run)
    CMD ["/bin/bash"]

    # You execute CMD with arguments on entrypoint (you pass arguments to it with run)
    ENTRYPOINT ["ls", "-l"]
