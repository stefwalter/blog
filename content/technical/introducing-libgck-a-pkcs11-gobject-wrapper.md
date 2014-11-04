Title: Introducing libgck: A PKCS#11 GObject wrapper
Date: 2010-10-04
Tags: technical, security, gnome
Slug: introducing-libgck-pkcs11-gobject

In gnome-keyring we use [PKCS#11][] for the storage of keys and
certificates. PKCS#11 is standard sort of a plugin API that allows
drivers or software to provide key storage and crypto algorithms to an
application.  
libgck is a GObject wrapper of PKCS#11. Still pretty low level but
makes PKCS#11 easier to use from GNOME or GTK+ apps. libgck is used
extensively in gnome-keyring and seahorse.  

-   GCK stands for "**G**object **C**rypto**K**i".
-   Currently lives in the gnome-keyring git module, but could be split
    into its own module in the future.
-   Replaces libgp11 with many lessons learned and a cleaner API.

Besides the mundane expected key and certificate storage functionality
and crypto mechanisms. There's support for stuff like [PKCS#11 URIs][]
used to identify keys or certificates residing in a certain key storage
or smart card. Also enumeration and loading of modules from a [common
system location][].  

All this goodness is in gnome-keyring master or 2.91.0

</p>

  [PKCS#11]: http://www.rsa.com/rsalabs/node.asp?id=2133
  [PKCS#11 URIs]: http://tools.ietf.org/html/draft-pechanec-pkcs11uri-02
  [common system location]: http://wiki.cacert.org/Pkcs11TaskForce
