Title: OCI Hook for Virtual Machines in Containers
Date: 2017-09-20
Category: Containers
Tags: linux, containers, kubernetes
Slug: oci-hook-vms-in-containers

In the [Cockpit Project](http://cockpit-project.org) we launch a crap ton of virtual machines.
This happens during testing of the Cockpit on various operating systems, browsers. We'll launch
about half a million virtual machines per month.

These VMs are launched by bots running in containers. In fact they launch the virtual machines
directly in the containers themselves. They're short lived VMs, boot in 10 seconds, perform
some tests and quit.

XXX kernel XXX

XXX privileged containers XXX

How does this work? Obviously containers can run QEMU in emulation mode, but that's slow.
We typically see that it's about 20 times slower to emulate a VM. But it turns out that the
```/dev/kvm``` kernel device used for running VMs is usable unprivileged as non-root. By
mounting that device in a container, containers can run virtual machines all they want.

Docker has a handy option for mounting ```/dev/kvm``` in a container. It looks like this.

In order to orchestrate the bots in the Cockpit Project, we don't use Docker directly. We use
Kubernetes ... and Kubernetes doesn't yet have an option to mount arbitrary devices in a
container. It does have options that bring nVidia GPU devices, but not much else.

OCI hooks to the rescue. These are are hooks documented by the Open Container Initiative
that let you run a process as a container is starting up.

I've written one such hook called oci-kvm-hook. All it does is bring the ```/dev/kvm```
device into each container launched.

Using this 
