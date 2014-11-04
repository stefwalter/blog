Title: Ditching Certificate Authorities with Convergence
Date: 2011-09-06 19:49
Tags: security, gnome
Slug: listened-to-moxies-talk-about-trust

Listened to [Moxie's][] [talk about Trust Agility and 'Convergence'][].
Sounds like a viable candidate for ditching the Certificate Authority
mess, or at least part of a solution. Go [watch the video][talk about
Trust Agility and 'Convergence'] if you haven't already.  
  
I was thinking about how we could implement support for
[Convergence][] in GNOME. The local cache of discovered valid
certificates would work far better across an entire desktop, rather than
each application (browser or otherwise) including code, configuration,
and storage to do it on their own.  
  
In fact it fits in nicely with the Trust Assertion stuff I've been
playing with. It'd be relatively straightforward to build a [PKCS#11 Trust Assertion module][] which used Convergence to seed Trust
Assertions. That would plug in easily to [GLib][], [GCR][], NSS etc.
(modulo a few patches not yet merged).  
  
But a few things popped into my mind while watching the video. Stuff
that it seems would need to be solved before this can be implemented as
general purpose solution. This is just off the top of my head, and
perhaps I'm woefully mistaken about something:  
  

1. **Protocols and Options:** Since I work for Collabora and we have a
    thing for XMPP, the first thing that stood out to me, is that the
    protocol is limited to HTTP, and more specifically SSL. Nearly every
    other protocol uses TLS for security.  
     
    So for starters this means that when the Notary wants to retrieve
    the certificate it'll need to know how to speak a given protocol. I
    guess different notaries could know speak different protocols when
    fetching the certificate from the target server?  
     
    Then the Convergence protocol would need to be changed to allow the
    caller to specify a well known protocol. Perhaps the protocol would
    also need some sort of capabilities support so that clients can know
    which Notaries can do what?  

2. **TLS Options, Extensions:** Taking this a step further, the Notary
    would need to provide the same TLS options and extensions that are
    used in the handshake. Most obviously: [SNI][]  

3.  **Hash Algorithm:** The protocol currently hardcodes things like
    SHA-1, which isn't aging very well. And as we've learned, [it's often better to pass the entire certificate around][]. Or at least,
    have it extensible where you can specify a hash algorithm to use.  

4.  Obviously Convergence doesn't (and probably can't) work if the
    server is requiring client certificates. I guess it's not meant to
    solve this use case though.  

5.  The installing of Notaries by downloading them as files from
    websites (including the certificate) smells sort of wrong, at first
    whiff. But if you figure that several Notaries would be distributed
    with a distribution (and could later be updated as necessary) and
    the certificates of websites hosting additional .notary files could
    be checked using the initial set of Notaries, then I guess it sort
    of makes sense.  

6.  **Protocol Specification:** Last but not least, the protocol needs to
    be documented and reviewed.

About usability: Obviously most users will never care about checking
which Notaries they use. I have a hard time expecting most people to
care. But they don't have to.  
  
The Trust Agility comes with the fact that a distributor like GNOME
could easily ship a default set of Notaries, and then update them as
needed without affecting functionality. As Moxie points out the browser
vendors can't even disable [screwballs like Comodo][] because it would
disable a quarter of the web. So that's where this is different. You can
replace screwed up Notaries without affecting functionality.  
  
And no more self-signed certificate warnings. Seriously, that dialog
(wherever it's implemented) is the worst security cop-out ever.

  [Moxie's]: http://thoughtcrime.org/about.html
  [talk about Trust Agility and 'Convergence']: http://www.youtube.com/watch?v=Z7Wl2FW2TcA
  [Convergence]: http://convergence.io/
  [PKCS#11 Trust Assertion module]: http://p11-glue.freedesktop.org/trust-assertions.html
  [GLib]: https://bugzilla.gnome.org/show_bug.cgi?id=656361
  [GCR]: http://developer.gnome.org/gcr/unstable/gcr-Trust-Storage-and-Lookups.html
  [SNI]: http://en.wikipedia.org/wiki/Server_Name_Indication
  [it's often better to pass the entire certificate around]: http://p11-glue.freedesktop.org/doc/pkcs11-trust-assertions/#justification-why-no-hash
  [screwballs like Comodo]: http://www.livehacking.com/2011/03/31/comodo-saga-continues-two-more/
