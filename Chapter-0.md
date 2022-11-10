# Chapter 0: Development Enviroment 
To have a controlled development environment and consequently have less chance of system-dependent errors, we will build an [OCI](https://github.com/opencontainers/image-spec/blob/main/spec.md) container image. The next step would be to run a container and map our development directory into the container's filesystem. You can pull the image from [docker.io](https://hub.docker.com/r/prealm/project-zero/tags) or build it from scratch using [alpine](https://www.alpinelinux.org/) [3.16.2 image](https://hub.docker.com/layers/library/alpine/3.16.2/images/sha256-1304f174557314a7ed9eddb4eab12fed12cb0cd9809e4c28f29af86979a3c870?context=explore). The choice is yours.

I use [Podman](https://podman.io/) (It's better suited for the steam deck, which is my daily driver ;) for the building and management of containers. Because Podman's interface is the same as Docker's, the commands will be the same for both. But the way they handle permission of mapped directories might be different. 

So we need Podman or Docker installed before proceeding. If you are using podman, replace "docker" with "podman" in the commands or add ```alias docker='podman'``` to your **.bashrc** and continue using the docker instead of podman.

We will build an x86 OS, and to keep everything simple, we will use an x86 version of alpine. Therefore, upon pulling the alpine image, you might encounter a warning. I will add spin-off projects for other ISAs in the future, and they will have their own OCI images. The tag of the images determines the host and target ISA. For example, x86-x86 is an x86 system that targets building an x86 OS. But amd64-arm is an amd64 host for cross-compilation of an arm OS.

### The easy way:
To pull the image from docker.io:
```
docker pull prealm/project-zero:x86-x86
```
### The fun way:
