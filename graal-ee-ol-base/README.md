# Building GraalVM EE Dockerfiles

## Notes

The make file uses the `--squash` flags to `docker build`. These are part of the experimental features of the docker client. Either switch these on
or remove the flag. 

## Setup

All installers for graalvm ee will need to be added to the  parent `installs` folder.

At a minimum you will need:

* Core GraalVM EE `tar.gz` file for your version of Java
* Matching LLVM jar file the version of graalvm and JDK being used
* Matching Native Image jar file the version of graalvm and JDK being used

## Building

Building the Docker Image is done through the use of the make file.
The make file can be paramterised to build a particular version of GraalVM EE with a particular JDK, though
obviously the `installs` directory will need to contain the appropriate installs.

To build, for example, GraalVM EE using JDK 11:

```
make GRAAL_VER=22.2.0 JAVA_VER=11
```

## Pushing to DockerHub

The repository that you push the built images to can be configured through the
`REPO` parameter. This is set in the `Dockerfile` to my repo name, but can be also set on the
command line.

```
make GRAAL_VER=22.2.0 JAVA_VER=11 REPO=you-repo-name
make push
```

## Running the Container

You can run the container, in order to test it, with the following:

```
make run
```