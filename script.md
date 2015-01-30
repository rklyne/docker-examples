
# Docker talk

Approximate script. Some detail missing.

## Why are we here
This talk is for people who have heard that Docker can solve all the problems they have with VMs but don't know where to get started.

I'm going to present the basics of what docker does, how to make it do those things and what to avoid doing. All this with a focus on those coming from a VM environment.

This is not a talk from a Docker expert. I am an expert in a few things, but Docker isn't one of them yet. I wrote this talk because I found the process of getting to this point more difficult than perhaps it should be and I wanted to bring some more people along with me.

## So, you've got problems with your VMs?

Of course you have. They are a heavyweight inefficient ans wasteful approach to encapsulating an operating environment.
They are not bad tools, they just don't offer what we often need when we reach for VMs.
They duplicate a lot of functionality of the host inside the VM - networking, logging, IO scheduling, firewalls, all the stuff an OS needs to keep a machine ticking over.
They take a lot of disk space.
They have inefficient process management and resource allocation.
They even make you specify up front how much ram your machine will need! Then either pre-allocate that much or take a huge hit every time the guest needs to allocate some ram.

### What does docker give us and how is it better?

Well, the problems I've just described are intrinsic to VMs. Docker doesn't have them and is therefore not a VM.

Docker is a new kind of beast. It's about 2 parts process manager, 1 part file system, 1 part package manager/repository.
And this does make it harder to get your head around. 

A virtual machien is a simple concept to convey. People are already familiar with machines, and a virtual one in no huge jump.
A docker container is less straightforward, so I'll need to explain first what a docker container is. I'll give the quick version of what's underneath, then fill it in with some examples.

(I will now start to go into detail on what a docker container is, with specific focus on how it's unlike a virtual machine.)

First off, Docker runs on Linux. It takes advantage of some neat kernel features to make the magic happen. This does mean that if you don't run Linux then it won't help you. Or it gives you a good reason to switch ;-)

For development on windows/mac you can run a pre-built vm called boot2docker. This will provide a 'docker' command in your host and link this up to a VM in virtualbox.

The main feature is cgroups (control groups), which enables a group of processes to be run under different rules to the rest of the system.
This allows a process to have a different view of the filesystem, mount table, the network, process list - everything that a process asks the kernel about can be modified to present a false view of the world. It even allows a cgroup process to run as root without compromising the host (unless you let it - more on that later).

Inside that structure, Docker will use an AUFS layered union file system for the data, will create private network adapters, set resource quotas/weightings and provide a common interface for managing these containers.

### The long game - composition/orchestration.
There's a lot that can be done once your applications are easily shippable units that can be transferred, installed and run automatically.

Composition - composing multiple docker containers into a service, e.g. an application server and a database.
Orchestration - deploying hoards of dockerised services, e.g. twenty copies of the same app with different configuration.
Automation - using a declarative config file (I want service X runnign at Y URL with Z config) to deploy your containers.

### It all sounds so easy....

This approach brings limitations. It only works with Linux. You need a command line to launch each program you need - ever done that with postgres/apache? No, you usually use the sysinit scripts to do it. At least we have those to copy.
All the techniques you've learned to manage a machine go out the window - we don't have a machine any more. These don't even have secureshell daemons (for good reason).

That's a lot of lifting done for you. It's also a significant set of changes to standard admin processes.


### Getting some containers running

(explain the command)

### Building your own containers

We'll start with a 'Hello World'. It's so simple as to almost explain itself, but I'll point out a couple of things anyway...

Then RUN - installing python in this case.



### Doing all the admin things differently

Explain connecting psql:
- over a network
- bash in processgroup
- psql in processgroup



.....





### Tools on top of docker

?????


A single Linux kernel for the guest and the host.
Partitioned using Linux cgroups - process groups with isolated permissions and view of the system.
This allows for fluid resource management - One kernel manages a set of processes and manages their resources as an OS normally would. For example, a docker process can request 100Gb of ram from the host and give it back ten seconds later. All normal.
Security for unknown code - the kernel keeps Docker containers isolated and prevents them from doing anything outside the ordinary - networking and processing.
Complete isolation - nothing can interact with a docker container unless you explicitly allow it.









