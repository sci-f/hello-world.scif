# The Scientific Filesystem

Hello World! This is the getting started example to build a Scentific Filesystem.
For maximum reproducibility, we will do this inside a linux container, both Docker
and Singularity.

[![https://img.shields.io/badge/hosted-singularity--hub-%23e32929.svg](https://img.shields.io/badge/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/458)

Read more about the [specification here](https://www.github.com/sci-f/sci-f.github.io/spec)
Read more about the [definition file](https://www.github.com/sci-f/sci-f.github.io/specification#recipe)

## Overview
The definition file [hello-world.scif](hello-world.scif) in this repository is the 
instruction that will be used to build the SCIF. Whether we are inside Docker or 
Singularity, we install the scif client as follows:

```
pip install scif
```

and you could also install from Github if you want a development version:

```
git clone https://www.github.com/vsoch/scif.git
cd scif && python setup.py install
```

The scif is then installed as follows (with root)

```
scif install hello-world.scif
```

For the instructions below, we will interact with the scif from inside the containers. If you want examples for interaction from outside of each container, see the [official documentation quick start](https://sci-f.github.io/tutorial-quick-start) that have more details. If you want to see the Docker and Singularity commands side by side, see the [really quick start](https://sci-f.github.io/tutorial-really-quick-start).

## Docker

First build your container:

```
docker build -t vanessa/hello-world-scif .
```

## What can I do?
If you don't know anything, you might execute the container. And then you discover the scif entrypoint to guide you!

```
docker run hello-world-scif

Scientific Filesystem [v0.0.4]
usage: scif [-h] [--debug] [--quiet] [--writable]
            {version,pyshell,shell,preview,help,install,inspect,run,apps,dump,exec}
            ...

scientific filesystem tools

optional arguments:
  -h, --help            show this help message and exit
  --debug               use verbose logging to debug.
  --quiet               suppress print output
  --writable, -w        for relevant commands, if writable SCIF is needed

actions:
  actions for Scientific Filesystem

  {version,pyshell,shell,preview,help,install,inspect,run,apps,dump,exec}
                        scif actions
    version             show software version
    pyshell             Interactive python shell to scientific filesystem
    shell               shell to interact with scientific filesystem
    preview             preview changes to a filesytem
    help                look at help for an app, if it exists.
    install             install a recipe on the filesystem
    inspect             inspect an attribute for a scif installation
    run                 entrypoint to run a scientific filesystem
    apps                list apps installed
    dump                dump recipe
    exec                execute a command to a scientific filesystem
```

## Apps
List apps

```
docker run vanessa/hello-world-scif apps
SCIF [app]              [root]
1  hello-world	/scif/apps/hello-world
```

## Help
Get help for hello world

```
docker run vanessa/hello-world-scif help hello-world
This is an example "Hello World" application. You can install it to a
Scientific Filesystem (scif) with the command:
scif install hello-world.scif
if you need to install scif:
pip install scif
```

## Run
Run "hello-world"

```
docker run vanessa/hello-world-scif run hello-world
[hello-world] executing /bin/bash /scif/apps/hello-world/scif/runscript
Hello World!
```

## Exec
Execute a command in context of the app "hello world." Notice how we replace the typical `$` for the environment variable with `[e]`

```
docker run vanessa/hello-world-scif exec hello-world ls [e]SCIF_APPROOT
[hello-world] executing /bin/ls $SCIF_APPROOT
bin
lib
scif
```

## Inspect
Inspect the hello-world app

```
docker run vanessa/hello-world-scif inspect hello-world  
{
    "hello-world": {
        "apprun": [
            "/bin/bash hello-world.sh"
        ],
        "appinstall": [
            "echo \"echo 'Hello World!'\" >> $SCIF_APPBIN/hello-world.sh",
            "chmod u+x $SCIF_APPBIN/hello-world.sh"
        ],
        "appenv": [
            "THEBESTAPP=$SCIF_APPNAME",
            "export THEBESTAPP"
        ],
        "applabels": [
            "MAINTAINER Vanessasaur",
            "VERSION 1.0"
        ],
        "apphelp": [
            "This is an example \"Hello World\" application. You can install it to a",
            "Scientific Filesystem (scif) with the command:",
            "scif install hello-world.scif",
            "if you need to install scif:",
            "pip install scif"
        ]
    }
}
```

## Shell
Interactively shell into the container in context of the app "hello-world"

```
docker run -it vanessa/hello-world-scif shell hello-world
[hello-world] executing /bin/bash 
root@ddf1ba27fd68:/scif/apps/hello-world# env | grep SCIF_
SCIF_APPMETA=/scif/apps/hello-world/scif
SCIF_DATA=/scif/data
SCIF_APPHELP_hello_world=/scif/apps/hello-world/scif/runscript.help
SCIF_APPDATA=/scif/data/hello-world
THEBESTAPP=$SCIF_APPNAME
SCIF_APPENV=/scif/apps/hello-world/scif/environment.sh
SCIF_APPROOT=/scif/apps/hello-world
SCIF_APPRECIPE_hello_world=/scif/apps/hello-world/scif/hello-world.scif
SCIF_APPDATA_hello_world=/scif/data/hello-world
SCIF_APPLABELS=/scif/apps/hello-world/scif/labels.json
SCIF_APPRUN=/scif/apps/hello-world/scif/runscript
SCIF_APPENV_hello_world=/scif/apps/hello-world/scif/environment.sh
SCIF_APPRUN_hello_world=/scif/apps/hello-world/scif/runscript
SCIF_APPLABELS_hello_world=/scif/apps/hello-world/scif/labels.json
SCIF_APPHELP=/scif/apps/hello-world/scif/runscript.help
SCIF_APPLIB_hello_world=/scif/apps/hello-world/lib
SCIF_APPS=/scif/apps
SCIF_APPMETA_hello_world=/scif/apps/hello-world/scif
SCIF_APPNAME_hello_world=hello-world
SCIF_APPBIN_hello_world=/scif/apps/hello-world/bin
SCIF_APPLIB=/scif/apps/hello-world/lib
SCIF_APPRECIPE=/scif/apps/hello-world/scif/hello-world.scif
SCIF_MESSAGELEVEL=INFO
SCIF_APPBIN=/scif/apps/hello-world/bin
SCIF_APPNAME=hello-world
SCIF_APPROOT_hello_world=/scif/apps/hello-world
```

## Pyshell
Start an interactive python shell to interact with the python client

```
docker run -it vanessa/hello-world-scif pyshell
Found configurations for 1 scif apps
hello-world
[scif] /scif hello-world
Python 3.6.3 |Anaconda, Inc.| (default, Oct 13 2017, 12:02:49) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.1.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: 
```

That's it for Docker! Next we review Singularity. Or just see the commands [side by side](https://vsoch.github.io/scif/tutorial-really-quick-start).

## Singularity

Again, first build your container!

```
sudo singularity build hello-world Singularity
```

## What can I do?
If you don't know anything, you might execute the container. And then you discover the scif entrypoint to guide you!

```
./hello-world 

Scientific Filesystem [v0.0.4]
usage: scif [-h] [--debug] [--quiet] [--writable]
            {version,pyshell,shell,preview,help,install,inspect,run,apps,dump,exec}
            ...

scientific filesystem tools

optional arguments:
  -h, --help            show this help message and exit
  --debug               use verbose logging to debug.
  --quiet               suppress print output
  --writable, -w        for relevant commands, if writable SCIF is needed

actions:
  actions for Scientific Filesystem

  {version,pyshell,shell,preview,help,install,inspect,run,apps,dump,exec}
                        scif actions
    version             show software version
    pyshell             Interactive python shell to scientific filesystem
    shell               shell to interact with scientific filesystem
    preview             preview changes to a filesytem
    help                look at help for an app, if it exists.
    install             install a recipe on the filesystem
    inspect             inspect an attribute for a scif installation
    run                 entrypoint to run a scientific filesystem
    apps                list apps installed
    dump                dump recipe
    exec                execute a command to a scientific filesystem
```

## Apps
List apps

```
./hello-world apps
SCIF [app]              [root]
1  hello-world	/scif/apps/hello-world
```

## Help
Get help for hello world

```
./hello-world help hello-world
This is an example "Hello World" application. You can install it to a
Scientific Filesystem (scif) with the command:
scif install hello-world.scif
if you need to install scif:
pip install scif
```

## Run
Run "hello-world"

```
./hello-world run hello-world
[hello-world] executing /bin/bash /scif/apps/hello-world/scif/runscript
Hello World!
```

## Exec
Execute a command in context of the app "hello world." Notice how we replace the typical `$` for the environment variable with `[e]`

```
./hello-world exec hello-world ls [e]SCIF_APPROOT
[hello-world] executing /bin/ls $SCIF_APPROOT
bin
lib
scif
```

## Inspect
Inspect the hello-world app

```
./hello-world inspect hello-world  
{
    "hello-world": {
        "apprun": [
            "/bin/bash hello-world.sh"
        ],
        "appinstall": [
            "echo \"echo 'Hello World!'\" >> $SCIF_APPBIN/hello-world.sh",
            "chmod u+x $SCIF_APPBIN/hello-world.sh"
        ],
        "appenv": [
            "THEBESTAPP=$SCIF_APPNAME",
            "export THEBESTAPP"
        ],
        "applabels": [
            "MAINTAINER Vanessasaur",
            "VERSION 1.0"
        ],
        "apphelp": [
            "This is an example \"Hello World\" application. You can install it to a",
            "Scientific Filesystem (scif) with the command:",
            "scif install hello-world.scif",
            "if you need to install scif:",
            "pip install scif"
        ]
    }
}
```

## Shell
Interactively shell into the container in context of the app "hello-world"

```
 ./hello-world shell hello-world
[hello-world] executing /bin/bash 
vanessa@vanessa-ThinkPad-T460s:/scif/apps/hello-world$ env | grep SCIF_
SCIF_APPMETA=/scif/apps/hello-world/scif
SCIF_DATA=/scif/data
SCIF_APPHELP_hello_world=/scif/apps/hello-world/scif/runscript.help
SCIF_APPDATA=/scif/data/hello-world
THEBESTAPP=$SCIF_APPNAME
SCIF_APPENV=/scif/apps/hello-world/scif/environment.sh
SCIF_APPROOT=/scif/apps/hello-world
SCIF_APPRECIPE_hello_world=/scif/apps/hello-world/scif/hello-world.scif
SCIF_APPDATA_hello_world=/scif/data/hello-world
SCIF_APPLABELS=/scif/apps/hello-world/scif/labels.json
SCIF_APPRUN=/scif/apps/hello-world/scif/runscript
SCIF_APPENV_hello_world=/scif/apps/hello-world/scif/environment.sh
SCIF_APPRUN_hello_world=/scif/apps/hello-world/scif/runscript
SCIF_APPLABELS_hello_world=/scif/apps/hello-world/scif/labels.json
SCIF_APPHELP=/scif/apps/hello-world/scif/runscript.help
SCIF_APPLIB_hello_world=/scif/apps/hello-world/lib
SCIF_APPS=/scif/apps
SCIF_APPMETA_hello_world=/scif/apps/hello-world/scif
SCIF_APPNAME_hello_world=hello-world
SCIF_APPBIN_hello_world=/scif/apps/hello-world/bin
SCIF_APPLIB=/scif/apps/hello-world/lib
SCIF_APPRECIPE=/scif/apps/hello-world/scif/hello-world.scif
SCIF_MESSAGELEVEL=INFO
SCIF_APPBIN=/scif/apps/hello-world/bin
SCIF_APPNAME=hello-world
SCIF_APPROOT_hello_world=/scif/apps/hello-world
```

## Pyshell
Start an interactive python shell to interact with the python client

```
 ./hello-world pyshell
Found configurations for 1 scif apps
hello-world
[scif] /scif hello-world
Python 3.6.2 |Anaconda, Inc.| (default, Sep 22 2017, 02:03:08) 
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 
```
