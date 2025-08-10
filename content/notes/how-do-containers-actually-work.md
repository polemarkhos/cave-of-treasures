+++
date = '2025-08-09T13:03:31+01:00'
draft = true
title = 'How Do Containers Actually Work'
summary = 'Putting some detail on the magic box abstraction.'
+++

Docker, podman, etc. are all working off the back of free kernel stuff.
1) the overlay filesystem (docker, maybe podman idk? def docker)
1) cgroups, namespaces (see man 7 cgroups, man 7 namespaces etc)

essentially building on the LXC linux container tech that was developed.
When docker makes an images, it pulls all the file layers as specified in the
Dockerfile, then it mashes them together into a union fs, also called an overlay
fs (see overlayfs in the kernel docs). then it chroots to the top of that little
fs and starts your process

To start a new process on linux you have to fork, exec and wait. But for a
containerised process you have to configure all the isolation and restriction
first. container runtimes create these "boxes" of isolation and restriction.
Containers are so widespread now there are even a lot of attempts at
standardisation, like opencontainers, kubernetes CNI CRI and CSI etc.

runc is widely used and can be used even without anything higher level. It was
created jointly by CoreOS, Google, Docker and others as a tool and reference
implementation of the Open Container Initiative runtime specification.

A container runtime is responsible for all the parts of running a container that 
isn’t actually running the program itself.

This all leads to an interesting thought - containers don't need images. Docker
introduced this alongside a bunch of other helpful stuff that saw it quickly
become the industry standard, but if you use runc you'll see that images don't
factor into it.

All runc needs is a bundle consisting of a config.json and a folder with the executable and any
necessary dependencies. This is why you can run "scratch" containers, without
any OS-like stuff at all. However often these bundle folders do contain a root
style filesystem with /var, /etc /proc etc
(use wagoodman/dive to inspect docker images)
Even docker run uses ephemeral containers to build an image!

Now you have a bunch of containers they need to play nice together. Docker or
even containerd are a good example of container managers, who handle pulling
images, unpacking bundles, container networking etc.

Containerd started as a component of docker, but became its own thing. Under the
hood it can use runc or any other runtime that implements the containerd-shim
interface.

the architecture is something like this:
```
[docker] [docker-compose] - command line clients using dockerd api
  v           v
[        dockerd        ] -  container engine daemon, handles higher level tasks
           |                 like login/build/inspect
           v
[        containerd     ] - this is what starts container runtimes, manages
            |               networks etc
            v 
[    containerd-shim    ] - this abstracts the low level runtimes
           |
           v
[        runc           ] - this actually implements the OCI container, preparing runtime for container, then exits.
```

The container is a regular linux process, which sees a virtualised OS around it.

So kubernetes - historically the kubelet used dockerd for launching groups of containers (i.e. pods), but this is deprecated now.
Now, kubernetes uses a more generic CRI, e.g. containerd or cri-o, the kubelet communicates with the container runtime using a gRPC client, and below that, as usual there is a CRI shim that sits on top of runc and the actual container processes on the node.

Actually, containers aren't simply processes. You can run multiple processes in a container if you want to. They are (obviously) CONTAINERS for processes. They are isolated, replicable environments to ensure your process runs the same way every time (among other things). Therefore we aren't restricted to using cgroups and namespaces for our actualy implementation of the container spec. We can even use VMs - anything goes as long as we hit the specification.
