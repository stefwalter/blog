Title: Talk at GUADEC on Integration of Certificate and Key Storage
Date: 2010-05-14
Tags: technical, security, gnome
Slug: talk-at-guadec-on-integration-on

I'll be attending GUADEC for the first time. Not only that but I'll be
giving a talk. I'm a bit nervous, but excited!

The talk is about integrating various
applications using keys and certificates to use a common key
storage.





  






Currently each application puts their
certificates and private keys in distinct locations, which make it hard
for the user, but also for application developers, since new
applications integrating crypto need to work out a whole raft of things
such as secure key storage.





-   Currently when you need to use a
    certificate with network-manager and a wireless connection, you have
    to specify three files in a fragile formats.
-   When using certificates with
    evolution or firefox or thunderbird each application stores them in
    their own key storage.
-   SSH Keys (which are in fact the same
    sort as the above) are stored in `~/.ssh`



It's a little bit like each application
not sharing a filesystem, but having their own part of the disk to write
their documents to. With GPG we have all applications sharing the same
keyring (per-user obviously), but so far this hasn't been the case with
X.509 certificates and keys.





  






Because of the development in
gnome-keyring around a standard called PKCS\#11 it's now possible to
integrate the key storage between applications, and in our talk we'll
discuss how to do this in a secure, transparent and configurable
way.







  






This also means it'll be easier for
applications to gain support for keys, and to have smart card related
features and other stuff like that in the future.</span>




