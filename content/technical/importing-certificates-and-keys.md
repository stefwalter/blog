Title: Importing certificates and keys
Date: 2011-10-05
Tags: technical, security, gnome
Slug: importing-certificates-and-keys

I've been working on an importer for keys and certificates that can work
with PKCS#11 key storage, such as smart cards, NSS or gnome-keyring.  
  
Here's a demo of it in action. If you want to try this out yourself,
you'll need:  

-   latest gcr library from [gnome-keyring git master][]
-   [p11-kit 0.7][] or later
-   [OpenSC configured][] to show up in p11-kit
-   [NSS configured][] to show up in p11-kit
-   an [OpenSC patch][] for mlock issue
-   an Entersafe based smart card, like the [Feitan 310 or 301][].
-   the smart card [needs to be initialized][]

It's possible that this works with other smart cards too, but I haven't
tested it. By the way, if you want to help work on smart cards support,
[Gooze gives away free smart cards][] for open source developers working
on this stuff.  

On to the demo...

<iframe allowfullscreen="" src="http://player.vimeo.com/video/30069077?title=0&amp;byline=0&amp;portrait=0" webkitallowfullscreen="" frameborder="0" height="477" width="720"></iframe>  

[View the Importer demo][] from [Stef Walter][] on [Vimeo][].

The importer and all the widgets are available for use by other apps in
the gcr library. So Seahorse has the same interface:

![Seahorse importer](images/seahorse-importer.png)

As you might note, I've been reworking the Seahorse user interface, more
about that coming soon...

  [gnome-keyring git master]: http://git.gnome.org/browse/gnome-keyring/
  [p11-kit 0.7]: http://p11-glue.freedesktop.org/releases/
  [OpenSC configured]: http://www.opensc-project.org/opensc/ticket/390
  [NSS configured]: https://live.gnome.org/CryptoGlue/Integration#NSS_libsoftokn3
  [OpenSC patch]: http://www.opensc-project.org/opensc/ticket/389
  [Feitan 310 or 301]: http://www.gooze.eu/
  [needs to be initialized]: http://www.gooze.eu/howto/smartcard-quickstarter-guide/smart-card-initialization
  [Gooze gives away free smart cards]: http://www.gooze.eu/feitian-pki-free-software-developer-card
  [View the Importer demo]: http://www.blogger.com/30069077
  [Stef Walter]: http://www.blogger.com/user6330669
  [Vimeo]: http://www.blogger.com/
