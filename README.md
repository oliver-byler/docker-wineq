[ Overview ]

This is a project to make a portable install for running older 3D games. It's currently intended for running P99 Everquest but can be tweaked for any game really. All that has to be changed is the WorkDir, mount paths, and the executable passed to wine :) 

[ Setup ]

First off, you'll need to install docker.io, if you are running a linux machine with nvidia, i suggest using nvidia-docker as it will run much better overall. Otherwise, just follow the recommended guides for installing non-nvidia linux, macOS, or windows. You will get the best results running on Ubuntu 18.04. I'm still farting around running it on the ubuntu shell on windows 10 and I'm sure it will work just as well. When you install docker, try to make sure its a global install with elevated privileges.

Once you got docker installed, you'll need to create a volume that will contain the Everquest install. I didn't include this in the docker container itself for a number of reasons, mostly because I'm not sure if its legal, but also to make patching easier as well as retaining player configs which would otherwise get blasted each time you start/stop. You can create the volume with the follow command:

    docker volume create everquest

This will make a directory within the docker install ( /var/lib/docker/volumes/everquest/_data on linux )

Copy your whole Everquest install to that directory which may require elevated privileges ( sudo )

[ Execution ]

Then all you have to do is invoke docker through the command line with the following:

    docker run -it \
    --rm \
    --env="DISPLAY" \
    --hostname="$(hostname)" \
    --mount source=everquest,destination=/root/.wine/drive_c/everquest \
    obyler/docker-wineq:base taskset -c 0 wine C:\\everquest\\eqgame patchme

This should fire up EQ after doing some winecfg stuff. It might load up a quasi weird fullscreen state ( will look like a little black window in the upper corner of the screen ). If so, just hit alt enter after it finishes loading to force it back into windowed mode. 3 of my machines were having issues with finding the display so you might need to mess around with mounting the display config for your OS, this meant passing in the xauthority cookie when on ubuntu 16.04 by adding the following parameters:

    --volume="${XAUTHORITY:-${HOME}/.Xauthority}:/root/.Xauthority:ro" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix" \

[ Docker Images ]

I've created a dockerhub repo that hosts image for several formats. I've made a base build ( obyler/docker-wineq:base ) that will literally work with any rig out there, regardless of hardware. It runs as poorly as you would expect, but its there to let people test/customize depending on their setup. If you've got nvidia, then use this image ( obyler/docker-wineq:nvidia ). If you've got amd use this one ( obyler/docker-wineq:amd )

I'm attaching the dockerfiles for each one as well so you can see the differences and/or play around as you like. Sorry for the brain dump on this as I'm running out of time and the power is back on so gotta cook din din.

Let me know if you run into any issues and I'll help ya figure em out. Docker is rad as fuck ahhaha. IF you really wanna try to optimize ( was doing this on my AMD rigs as they were a bit whacky ), look into winetricks and play around the different versions of directx and windows OS versions.

If you just wanna poke around as if you were ssh'd into the machine then just run a shell on the container with /bin/bash:

    docker run -it \
    --rm \
    --env="DISPLAY" \
    --hostname="$(hostname)" \
    --mount source=everquest,destination=/root/.wine/drive_c/everquest \
    obyler/docker-wineq:base /bin/bash
