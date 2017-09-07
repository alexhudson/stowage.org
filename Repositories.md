# Repositories

stowage takes an intentionally simple approach to repositories, but if you've used rpm or dpkg and their attendant tools, some of this will feel familiar.

The use case here is to make available a series of tools from one place which can be searched over. This enables teams to share tools with each other, with a standard approach for making them available.

If you read [Quick start introduction](Quick_start.md) then you've already seen the basics in action, so let's start by talking about creating a repository first.

## Creating a repository

To begin with, we need a single specfile to create a repository, although the more we have, the more useful the repository. stowage ships with a directory of example specfiles, so I'm going to demonstrate using this. At the time of writing, the example directory looks like this:

```
$ ls examples/
dockerfile-lint.json  hello-world.json  microsoft-dotnet.json
```

In order to create a repository we need to create the master list of specfiles, and then publish this whole thing so that it's usable.

The actual creation process is done with stowage:

```
$ ./stowage repo-scan ./examples/
$ cat ./examples/stowage.json 
{
	"Name": "",
	"URI": "",
	"Description": "",
	"Listing": [
		{
			"Name": "dockerfile-lint"
		},
		{
			"Name": "hello-world"
		},
		{
			"Name": "microsoft-dotnet"
		}
	]
}
```

As you can see, all it does it create a JSON file with a few fields that describe the repository and the contents within. You have a few fields that need to be completed as well:

* the repo needs a Name; this should be short and sweet. When a user adds this repo to their stowage, this is the name it will be known by.
* the repo ideally should have a description as well.

The URI field can be set to the location the repo will be available, but this will get filled in by stowage when it's added - you don't need to fill this in.

From time to time, you will want to update the definition of the repository. Since you don't want to fill out Name and Description each time, stowage will load any existing scan and re-use that information - this is a one-time job!

## Publishing a repository

As you've seen, a repository is just a directory, with a main listing file called `stowage.json`, and then a series of specfiles for all the commands within the repository.

This means it's pretty easy to host; all you need is a webserver capable of serving up static content. It's possible to do this with github pages, or a self-installed nginx, and everything inbetween.

It's up to you how you want to do this - but standard tools like `rsync` or `aws-cli` can do synchronise your local repo to your webserver.

Then, all you need to do is share the URL for the repo. stowage will automatically add the `stowage.json` piece of the URL, so you don't need to provide that.

There's an example repo here:

  http://stowage.org/repo/demo/

## Using a repostory

Once you have a repository published, any user with access to the URL is able to install it. In the case of the demo repo above, we can add it to stowage like so:

```
$ stowage repo-list
$ stowage repo-add http://stowage.org/repo/demo/
$ stowage repo-list
stowage-demo
```

With at least one repo installed, we can then start searching them for interesting commands:

```
$ stowage search hello
stowage-demo::hello-world	
$ stowage install stowage-demo::hello-world
hello-world installed
$ hello-world 

Hello from Docker!
...
```

Ok, maybe not such an interesting command - but we can see how it's now easy to create some wrappers of containers, and use stowage repos to share those with others in way they're easy to discover and then install.
