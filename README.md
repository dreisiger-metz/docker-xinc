# xinc : Running X11 Clients Inside a Container
This work-in-progress combination of Dockerfiles and shell script provides a simple way to run X11 (and PulseAudio) client applications inside of a container. (Simple, when compared to the likes of https://github.com/mviereck/x11docker)


## Getting Started
First, build whichever image(s) you're interested in, using the following commands as an example:

```
$ docker build -t xinc:ubuntu-20.04 ubuntu-20.04/
$ docker build -t xinc:debian-11 debian-11/
```

To _run_ an xinc container, with all of the socket, directory and UID mappings handled for you, simply execute the `xinc` shell script. This will open a new xterm, from which you can run, e.g., `screen` or `firefox`; if you'd rather launch a different client automatically, you can do so via command line arguments in the usual way.


To see what's happening 'under the hood', you can, of course, simply look at the helper script, but a stripped-down version of what it's doing is shown below:

```
$ docker run -v /tmp/.X11-unix:/tmp/.X11-unix \
             -e DISPLAY=$DISPLAY \
             -v /run/user/${UID}/pulse/native:/var/run/pulse/native \
             -e PULSE_SERVER=unix:/var/run/pulse/native \
             --mount type=bind,source="${XINC_HOME}",target=/home/default \
             -it --rm --user=${UID} --workdir /home/default xinc:${XINC_TAG} "$@"
```



## References
This solution was inspired by approaches outlined in the following pages (amongst others):

  - http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
  - https://stackoverflow.com/questions/28985714/run-apps-using-audio-in-a-docker-container/28985715
