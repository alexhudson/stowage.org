# Creating new images

This is the primary use case that I'm interested in: being able to package up different types of tools in a Docker image has allowed me to create and quickly share tools with others. I think the specific benefits are:

* ease of use. You can create a container, push it, and someone else can `stowage install` it - it's immediately available.
* re-enforces "toolkit" pattern. New users of Docker often put a lot of build process in their Dockerfiles, bloating them in size. Use of toolkit type images like these improves best practice and results in smaller leaner images.
* simple to share with CI. You hopefully have some integrated CI/CD system, like Jenkins or similar. These tools can be used there as well, and this ensures the development environment is close to the CI environment: you can be sure you have the same versions, for example.

## Example: creating a webpack tool

Obviously, if you're an NPM user, it's pretty easy to do `npm install ...` and get the tools you need. However, `npm install -g` (global install) is only really good for developer environments, but a local install may not be in `$PATH`. Also, if you're running on a Mac, any extensions with native components will work locally but not in your container, and vice-versa - creating differences where you don't want them.

The toolkit approach here is to create a container image that contains the tool, plus any dependencies needed. This can be used directly, and stowage gives you the ability to use this as a binary.

In this example, I'm going to package up `webpack` in an image so that I can use it standalone.

### Building the tool

The [Dockerfile](example1/Dockerfile) for this example is very simple:

```
FROM node

RUN npm install -g webpack webpack-dev-server yarn

LABEL "org.stowage.spec"="{ \
    \"Name\": \"webpack\", \
    \"Command\": \"webpack\", \
    \"Mounts\": [ \
        { \
            \"Cwd\": true, \
            \"Guest\": \"/workspace\" \
        } \
    ] \
}"

VOLUME /workspace
```

I'm basing this image on the official node image, and then installing webpack. `/workspace` is a directory that will be used to project the current working directory when I run it.

In the middle, I've put a specfile in the recipe using the `org.stowage.spec` custom label. Let's talk about what this is doing quickly:

* `Name` tells stowage what filename the binary wrapper should have - I'm saying `webpack` here because I just want to run it the same way I would be able to if I had done `npm install -g`
* `Command` tells stowage what command to run in the image. This is equivalent to `COMMAND` in the `Dockerfile`, and depending on how you define the `ENTRYPOINT` it may not be needed
* `Mounts` tells stowage which volumes to mount at runtime. Here, there is only one - I want to mount the current working directory as `/workspace`

I've built this and published as a [test image on Docker Hub](https://hub.docker.com/r/ealexhudson/stowage-webpack/), although I don't necessarily recommend using it as-is!

### Installing the tool

Ok, we have the container image created. I've pushed it to Docker Hub, although it doesn't need to be pushed anywhere unless you plan to share it.

This can be installed with stowage the same way any other container would be:

```
$ stowage install ealexhudson/stowage-webpack
webpack installed
$ stowage list
webpack
```

We can see the first difference: if we had not included the stowage specfile in the container label, our binary would be called `stowage-webpack`. That could be ok, but I want it to be called `webpack` and have said so in the specfile - so that's what we have.

I can now test this quickly and make sure it works in basic fashion:

```
$ webpack --version
3.5.5
```

Perfect! It's installed and executable, other people on my team could install it exactly the same way, and we're all then definitely using the same tooling - plus our CI. Not just that, I can push out new versions with additional capabilities or libraries, and it's super-simple to keep everyone up-to-date.
