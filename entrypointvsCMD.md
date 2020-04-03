### ENTRYPOINT
`ENTRYPOINTFormat and RUNinstruction format as divided execform and shellformat.`

ENTRYPOINT It may be replaced at run-time, but compared CMD need to pass docker runparameters --entrypoint to be specified.

When specified ENTRYPOINT, the CMD meaning of change occurs, it is no longer a direct operational command, but the CMD contents as an argument to ENTRYPOINTthe instruction, in other words when the actual implementation, will become:

Scene 1: Let the image be used like an image command
Suppose we learned that he needs a mirror image of the current public network IP, it can first CMDbe achieved:

### Dockerfile for ip in cmd 
```FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "https://www.showmyip.gr/" ]```


`If we use docker build -t myip .to build a mirror, and if we need to query the current public network IP, only need to do:`

$ docker run myip
Well, it seems like this can be used directly as a command, but the command always has parameters, if we want to add parameters? From the above example CMD you can see the essence of command curl, so if we want to display the HTTP header information, you need to add -iparameters. Then we can directly add -iparameters to docker run myipit?

```$ docker run myip -i
docker: Error response from daemon: OCI runtime create failed: container_linux.go:345: starting container process caused "exec: \"-i\": executable file not found in $PATH": unknown.
ERRO[0001] error waiting for container: context canceled
So if we want to join -ithis argument, we must re-enter the complete command:```

$ docker run myip curl -s https://www.showmyip.gr/ -i
This is obviously not a good solution, but using ENTRYPOINTit can solve this problem. Now that we use ENTRYPOINTto achieve this image:

FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "https://www.showmyip.gr/" ]

This time we will try to use it directly docker run myip -i:

It can be seen that this time it was successful. This is because when there is ENTRYPOINT, the CMDc ontent will be passed as a parameter ENTRYPOINT, and here -i is the new CMD, and therefore passed as a parameter curlto achieve our desired results.
