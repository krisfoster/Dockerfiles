# Dockerfiles

Folowing is a collection of Dockerfiles for building GraalVM
Docker images. There is a directory for each build, within which
is a `'Dockerfile` and a `Makefile`.

To build an image:

```
$ make
```
To run the image:

```
$ make run
```

To push the image to my repository:

```
$ make push
```

## Installs Directory

The various Dockerfiles depend on an installs directory, within this
folder, being present. This will need to contain all of the installs for
GraalVM EE, but as their is no way of downloading these automatically at
the moment you need to download these yourself from OTN.

**YOU NEED TO DOWNLOAD THE GRAAL EE INSTALLERS ETC YOURSELF. YES, THAT MEANS YOU**

## Current Dockerfiles

### GraalVM EE WIth Oracle Linux 7

### GraalVM EE With Ubuntu