# Stowage

[Stowage](https://github.com/alexhudson/stowage) is a package manager-alike for software packaged in Docker images. Stowage makes it simple to run software locally, and also create and share it with others - it's great for distributing tools in development teams, and for simplifying CI/CD processes.

Using Stowage, you can "install" definitions of commands - it creates a simple wrapper which allows you to call the container as if it were a local binary:

```
$ stowage install hello-world
$ hello-world 


Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

## Install Stowage

Visit the [stowage releases](https://github.com/alexhudson/stowage/releases/latest) and download the latest binary for your operating system.

Alternatively, if you're on a Mac, you can `brew tap alexhudson/stowage && brew install stowage`

Or, if you're on Linux, you can `sudo docker run ealexhudson/stowage get-started | sudo sh`

## Use Stowage

* [Quick start introduction](Quick_start.md) for a speedy tour of stowage and why you might want to use it
* [Existing Docker images](Existing_images.md) can be used with little or no adapation
* [Creating Docker images](Creating_images.md) to run binaries is the common use case which stowage was created for
* [Using and creating repositories](Repositories.md) enables you to share stowage specs with others easily
