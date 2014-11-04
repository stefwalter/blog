Title: Viewer for Certificate and Key files
Date: 2011-09-01
Tags: technical, security, gnome
Slug: viewer-for-certificate-and-key-files

So a lot of the work I do doesn't have any user interface. The best user
interface is no user interface, well one that isn't needed. But recently
I've been working some tools to view the plethora of certificate and key
formats out there. So I couldn't resist blogging about this, because I
get to use screenshots, heh.  
  
The [NLnet Foundation][] has been sponsoring Collabora to do some smart
card work, and this is part of that. This work is part of the [GCR
library][], and is merged into gnome-keyring master.  
  
Here's a certificate viewer showing a certificate:  
  
![Certificate viewer](images/certificate-viewer.png)

Details can then be expanded:

![Certificate viewer](images/certificate-viewer-1.png)

And here's what a key looks like.

![Certificate viewer](images/certificate-viewer-2.png)

When the file is locked (like a PKCS#12 file) it can be unlocked like
this video shows. It also shows the appalling state of affairs with
hundreds of certificate authoritities, dubious and otherwise.

<iframe width="640" height="480" src="//www.youtube.com/embed/N67Zl6Zlx_g" frameborder="0" allowfullscreen></iframe>

In the next release, we'll have an "Import" button in the bottom right
corner, so that the certificates and keys being viewed can be imported
and used [into common locations][]. Hopefully we'll also get able to
view PGP keys in files (before importing them).

The widgets you see displaying the certificates can be used by any
application from the GCR library. 

  [NLnet Foundation]: http://nlnet.nl/
  [GCR library]: http://developer.gnome.org/gcr/unstable/
  [into common locations]: https://live.gnome.org/CryptoGlue
