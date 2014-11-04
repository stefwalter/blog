Title: About Trust Assertions
Date: 2010-10-13
Tags: technical, security, gnome
Slug: about-trust-assertions

I've been working on some specifications for storage of 'trust'. This a
sufficiently vague and abstract concept to require a hoity toity name:
*Trust Assertions*  
  
Trust assertions are used to assign an explicit level of trust to a
public key or certificate. I'll just refer to certificates below because
that's the easiest to grasp, but the concept is sufficiently abstract to
allow trust assertions for other types of keys.  
  
Examples of trust assertions include:  

-   Certificate Authority root certificates
-   Certificate Revocation Lists
-   Certificates you decide to trust manually (for your favorite self
    signed certificate)
-   Certificates marked bad explicitly

Trust assertions are not about the process of deciding whether you'll
eventually trust a certificate. Ultimately an application needs to
determine trust a certificate for a given connection, email or instant
message. It does this by checking if it's valid, who it's signed by, and
an obscene amount of other rules. It usually does this with the help of
a crypto library.  
  
Only the application has all the information necessary to make a trust
decision. A simple example of this is how web browsers check that the
common name (ie: `CN`) of the certificate matches the domain name of the
`https://` website you've browsed to.  
  
Trust assertions are about storing basic facts that applications use in
their trust decision process.  

 **The Concept** 

A trust assertion describes a level of trust in a certificate for a
given usage or purpose.  Conceptually each trust assertion is a triple
containing:  

-   Certificate Reference
-   Usage (aka purpose)
-   Level of Trust

We examine each of these parts of the triple in further detail below.  
  
**The Level of Trust**  
  
A trust assertion ultimately denotes a level of trust. These are:  

-   Untrusted: The certificate is explicitly untrusted.
-   Unknown: The trust is not known and should be determined
    elsewhere.
-   Trusted: The certificate itself is explicitly trusted.
-   Trusted Delegator: The certificate is trusted as a certificate
    authority trust root.  Trust is conferred to certificates that
    this certificate has signed, or that signed certificates have
    signed, and so on.

**The Usage**  
  
A trust assertion always refers to a specific purpose or usage.  A
certificate may be trusted for purposes like: email, code signing,
authenticating a server.  
  
It turns out that carte blanche trust not a super useful concept. You
(should) always trust someone for some purpose. You trust your bank
with your money, not your children; you trust your school with your
children, and so on.  
  
**The Certificate Reference**  
  
And finally we have the certificate that the trust assertion refers
to.

  
Pretty boring stuff actually. But it does get exciting. By comoditizing
trust storage, we can use these well defined concepts for new methods of
trust decision making.  
  
The way certificate authorities work in your web browser scares a lot of
people. By changing to a more general trust storage model, we have the
possibilities for applications to try out new trust paradigms. One
example is the have-I-seen-this-key-at-this-site-before trust model,
used by OpenSSH. But I'm certain that more methods will emerge as more
energy is brought to bear on the problem.  
  
Okay enough hand waving, and back to earth.  
  
The [specification I'm working on][] defines how to store trust
assertions as PKCS\#11 objects. This isn't a new concept, and has been
[implemented in Mozilla's NSS][] for a long time. However as far as I
can tell it hasn't yet been documented.  
  

<div
style="margin-bottom: 0px; margin-left: 0px; margin-right: 0px; margin-top: 0px;">

After some prodding (thanks Nikos) I figured I'd do some work to
document it properly.

</div>

<div>

</div>

  
GNOME Keyring is completing its implementation of trust assertions. For
a long time now, we've had simple read-only trust assertions that
exposed everything in <span class="Apple-style-span"
style="font-family: 'Courier New', Courier, monospace;">/etc/ssl/certs</span> as
a trusted delegator (ie: a certificate authority).  
  
But now we're working on rounding out the support on the [trust-store
branch of gnome-keyring][].  
  
[Cosimo][] is working on XTLS to encrypt jabber chats in empathy, and so
the trust-store work will help store certificate exceptions in
gnome-keyring.

</p>

  [specification I'm working on]: http://thewalter.net/git/cgit.cgi/pkcs11-trust-assertions/tree/draft-pkcs11-trust-assertions.xml
  [implemented in Mozilla's NSS]: http://mxr.mozilla.org/mozilla-central/source/security/nss/lib/util/pkcs11n.h
  [trust-store branch of gnome-keyring]: http://git.gnome.org/browse/gnome-keyring/tree/?h=trust-store
  [Cosimo]: http://blogs.gnome.org/cosimoc/
