# Quick start

This assumes that you have stowage installed and working locally - if not, please head back to the homepage and do that first. There are a few different options for different platforms.

I'm going to use the demonstration repository, as this is the quickest way to show stowage. Repositories are covered in more detail in later documents.

## Add the demo repository

Before we do anything, let's tell stowage about the repository and make it available:

```
$ stowage repo-add http://stowage.org/repo/demo/
$ stowage repo-list
stowage-demo
```

The `stowage-demo` repository has been added. We can now search for commands that have been added to this repo: in this demo repo, we've published the example specifications. One of them is the Dockerfile lint utility from project atomic. We can find it with `search`:

```
$ stowage search lint
stowage-demo::dockerfile-lint
```

## Install a command

Once a command is found, it can be installed. When installing from a repo you need to specify that as well - however, there are other arguments you can supply to install that we'll look at later.

```
$ stowage install stowage-demo::dockerfile-lint
dockerfile-lint installed
```

*NB* You may need to use `sudo` in order for stowage to correctly install the wrappers in the correct location - right now, it tries to go into `/usr/local/bin` so that they end up in your `$PATH`. In places, stowage isn't very graceful about dealing with lack of permissions - sorry!

Now that the command has been installed, I can use it on unsuspecting Dockerfiles. I'm going to lint the Dockerfile distributed with stowage:

```
$ dockerfile-lint ./Dockerfile

--------INFO---------

INFO: There is no 'EXPOSE' instruction. Without exposed ports how will the service of the container be accessed?. 
Reference -> https://docs.docker.com/reference/builder/#expose
```

## What's happening under the hood?

Above, we installed a command and were able to run it - but we know that the command is actually a Docker image. What's going on?

So first, this obviously doesn't work with all images - only those deliberately designed to be usable with `docker run` in this manner.

There is only a small shell wrapper installed on the system to allow the use of the image. It's effectively just calling `docker run` for us; in this example it's something like:

```
docker run --rm -t -i --privileged -v `pwd`:/root projectatomic/dockerfile-lint dockerfile_lint "$@"
```

Now, of course, we could just run that command directly. We could also manually write wrappers like these - which is what I used to do - or also use shell aliases.

All of these are possible, but the point of stowage is to try to give a bit of an abstraction here - the specfiles that stowage use ensures it runs the right parameters, and means you don't have to maintain wrappers so manually or remember the different docker invocations.
