Title: VMWare Player on Fedora 16
Date: 2011-10-28
Tags: technical, security
Slug: vmware-player-on-fedora-16

I have some VMWare VM's I've been using here and there. I probably
should convert them to Virtual Box, but I've had a rough time getting
that working as well.  
  
So ... every time you upgrade the kernel, VMWare barfs because kernel
headers have changed. Usually I look around for patches to the VMWare
sources, but this time there were none I could find, so I figured it was
my turn.  
  
[This simple patch][] makes VMWare Player 4.0.0 work with Linux 3.1.0.
At least it seems to work. What I did to patch it in:  
  
    :::sh
    $ mkdir /tmp/vmware
    $ cd /tmp/vmware
    $ wget http://thewalter.net/stef/misc/vmnet-4.0.0-linux-3.1.0.patch
    $ tar -xvf /usr/lib/vmware/modules/source/vmnet.tar
    $ patch -p0 < vmnet-4.0.0-linux-3.1.0.patch
    $ sudo cp /usr/lib/vmware/modules/source/vmnet.tar /usr/lib/vmware/modules/source/vmnet.tar.bak
    $ sudo tar -cvf /usr/lib/vmware/modules/source/vmnet.tar vmnet-only
  
And then run vmplayer and let it do its install thing. It says that the
services fail to start (systemd incompatibility), but it works
regardless.  
  
Note: If you try this and it doesn't work for you (or makes your doggy
sad), don't complain to me. [Complain to VMWare][].


  [This simple patch]: http://thewalter.net/stef/misc/vmnet-4.0.0-linux-3.1.0.patch
  [Complain to VMWare]: http://communities.vmware.com/index.jspa
