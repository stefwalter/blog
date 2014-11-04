Title: Implemented trust assertions and certificate chains
Date: 2010-12-11
Tags: technical, security, gnome
Slug: implemented-trust-assertions-and

  
Trust assertions are bits of trust information used by applications to
make trust decisions about certificates. For example, trust assertions
can represent certificate authority anchors, pinned certificate
exceptions, or revocation lists. Trust assertions do not represent the
trust decision itself, but they're used in a trust decision.  
  
By using trust assertions applications (and libraries) can make
consistent trust decisions and not confuse the poor user with different
security in each app when making TLS connections.  
  
For example all the applications on the user's desktop would use the
same set of certificate authorities when making TLS connections. And the
user can then easily manage that set of certificates. It's also easy to
store per-host pinned certificate exceptions for self-signed
certificates, and have all applications use them consistently.  
  
I've put together a [spec for storing and looking up trust assertions
via PKCS\#11][] which allows a loose coupling between applications and
the storage of these trust assertions. I've also implemented support for
storing trust assertions in Gnome Keyring, and [client side support in
libgcr][].  
  
To make it all very easy to use, I've added a [GcrCertificateChain][]
class which builds up a certificate chain, based on trust assertions and
gets it ready for verification by your favorite crypto library.  
  
All this goodness is available in the [trust-store branch][] of
gnome-keyring, and it looks like [empathy will be the first][] app to
make use of it. I'm gonna try and see how we can fit this into the nice
new [GTlsConnection][] support in glib.  

I'm looking forward to the [security devroom at FOSDEM][] and hope to
talk about some of this stuff.

  [spec for storing and looking up trust assertions via PKCS\#11]: http://people.collabora.co.uk/~stefw/trust-assertions.html
  [client side support in libgcr]: http://people.collabora.co.uk/~stefw/gcr-docs/
  [GcrCertificateChain]: http://people.collabora.co.uk/~stefw/gcr-docs/GcrCertificateChain.html
  [trust-store branch]: http://git.gnome.org/browse/gnome-keyring/log/?h=trust-store
  [empathy will be the first]: https://bugzilla.gnome.org/show_bug.cgi?id=636258
  [GTlsConnection]: https://bugzilla.gnome.org/show_bug.cgi?id=588189
  [security devroom at FOSDEM]: http://opensc-project.org/opensc/wiki/FOSDEM2011
