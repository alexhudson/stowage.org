# Using existing Docker images

You may already have an existing Docker image that you want to use with stowage - that's great! 

As a convenience, stowage will attempt to recognise Docker image references when you install - meaning that any registries you have setup should automatically work, and Docker Hub will work by default.

For an example, let's look at a command which doesn't have a specfile shipped as an example, or in a repo: the MS Azure CLI.

```
$ stowage install microsoft/azure-cli
azure-cli installed
```

I now have a local command `azure-cli` available to me, as the last piece of the image name is taken as the name of the command.

In this case, the image is designed to give you a command line from which the `azure` command can be run. We can verify this by asking for the version:

```
$ azure-cli azure -v
0.10.13
```

So we're installed and working! 

## Altering the specfile

It would be _even more convenient_ if we didn't have to specify what feels like the same thing twice - ideally, we just one command.

This is why stowage has specfiles. Although the image is directly usable in most instance, it isn't always - or, the usage can be improved. This is one of those cases.

stowage creates a default specfile on installation. We can just pinch that as our starting point - in this case, it's `/usr/local/share/stowage/spec/azure-cli`.

Copy this locally as `azure-cli.json`. Add `azure` as the `Command` in the file, and this will then solve our problem.

As well as giving images to install, we can also use the specfiles themselves:

```
$ stowage install ./azure-cli.json
azure-cli installed
$ azure-cli help
info:    Executing command help
info:             _    _____   _ ___ ___
info:            /_\  |_  / | | | _ \ __|
info:      _ ___/ _ \__/ /| |_| |   / _|___ _ _
info:    (___  /_/ \_\/___|\___/|_|_\___| _____)
info:       (_______ _ _)         _ ______ _)_ _ 
info:              (______________ _ )   (___ _ _)
info:    
info:    Microsoft Azure: Microsoft's Cloud Platform
info:    
info:    Tool version 0.10.13
help:    
```

Even easier!
