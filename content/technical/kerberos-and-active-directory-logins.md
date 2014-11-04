Title: Kerberos and Active Directory Logins
Date: 2012-06-15
Tags: technical, security, gnome
Slug: kerberos-and-active-directory-logins

Ray and I and some others have been working on making it easy to use
Kerberos single sign on with GNOME 3.6. The feature itself isn't super
revolutionary. You sign in with your realm login (eg: your Active
Directory user name and password) and then you can go on and use other
services with that same kerberos sign in.

You could already do this, but setting it up was hard ... and setting it
up so it couldn't be trivially hacked was even harder. If you just use
pam_krb5 without 'enrolling' your machine an attacker can log in as
anyone (woooooo). ... and keeping it running without breaking down on
you was even harder than that, especially for mobile environments.

So I've been working a lot on making this easy to setup; auto
discovering the domain/realm, its settings and as much information as
possible. Last week [some user visible][] changes [landed into
gnome-control-center][] so here are some screenies.

You'll notice the new *Enterprise Login* mode of the *Add account*
dialog:

![Add account](images/1-add-account.png)

It lets you add users from an Active Directory domain or [FreeIPA][]
(soon) realm, and perhaps others in the future.  Any domains we already
know about are in the drop down. If this is the first time you're using
the feature, we try and automatically discover the domain from your DHCP
DNS domain name. We automatically discover what kind of realm/domain
we're dealing with, all the other settings, and whether it's valid or
not.

Login details for the user are typed in, and then we bonk the *Add*
button:

![Enterprise login](images/2-enterprise-login.png)

We try to be intelligent about validation and give good feedback:

![Validate domain](images/3-validate-domain.png)

![Validate login](images/4-validate-login.png)

![Validate password](images/5-validate-password.png)

If the domain requires administrative credentials to enroll the local
machine, then we prompt for those. Active Directory on Windows 2003 and
later [don't require admin creds by default][]. FreeIPA does.

![Administrator](images/6-administrator.png)

And voila, the user is added. Only the users specifically added should
be able to log in; there's actually a bit of work to do on that, but
that's the plan. If an admin wants to enable any domain user to log in
on the machine, then they can enroll the machine using the command line
or admin tools. Both are supported, just not both in
gnome-control-center.

Here I added two users, Fry and Leela. Not sure why one of them showed
up with their full name and the other not. Something to fix...

![Added accounts](images/7-added-accounts.png)

Under the hood a the enrollment in realms is managed by a small DBus
on-demand system service called [realmd][]. realmd can also be driven
using command line tools, which expose additional options.

It was important to me to make sure that the diagnostics are clear. So
many of these tools throw up cryptic error messages without anywhere to
go to figure out 'Why?'. So we try hard to have user visible error
messages, and then very clear diagnostic output on the console when
performing these operations, that looks like:

    :::text
    * Looking up our DHCP domain
    * Searching for kerberos SRV records for domain: _kerberos._udp.ad.thewalter.lan
    * Searching for sub zone on domain: _msdcs.ad.thewalter.lan
    * dc.ad.thewalter.lan:88
    * Found AD style DNS records on domain
    * Successfully discovered realm: AD.THEWALTER.LAN

Hmmm, what else ... Ray's been working on other parts of this: fixing
the user accounts panel so it makes sense for non-local users. And
making sure that the Kerberos tickets we get at login are correctly
renewed and reauthenticated.

### SSSD

These guys. Props to the SSSD guys. They've been working hard to make
that daemon the perfect client for Kerberos/IPA/AD. It's a modern clean
implementation, and makes stuff like using your domain login when not
attached to the domain really reliable and easy.

### No mo' NTP Time Syncing

I've been running around the kerberos stack trying to fix issues that
cause configuration problems, fragility or just plain nastiness. For
example:

For decades now kerberos implementations have pretty much required you
to sync your time up with the kerberos server. It makes you roll your
eyes that a security protocol relies so much on time for security, when
the syncing of that time is almost always insecure. But oh well.

Anyway, for the good news. 15 years ago [these guys figured out][] how
to do kerberos without time syncing. And bit by bit their ideas have
made it into kerberos implementations, but nobody new about it because
the there were tons of fiddly bits that made assumptions about time
syncing. I did a few last patches to sort out some issues, which have
been accepted by MIT Kerberos.

Anyway, that was long enough, there's lots more, but it's starting to
get boring ...

Lots of patches are being merged as we speak, but if you want to test
this stuff out right now, ping me on IRC \#gnome-os on gimpnet, and I'll
help you get started.

  [some user visible]: https://live.gnome.org/Design/Proposals/UserIdentities?action=AttachFile&do=get&target=user-accounts-add.png
  [landed into gnome-control-center]: https://bugzilla.gnome.org/show_bug.cgi?id=677548
  []: http://2.bp.blogspot.com/-YAnYRd1vlMI/T9sdGxrDUiI/AAAAAAAABXI/yh4uS9doQ5k/s1600/1-add-account.png
  [FreeIPA]: http://freeipa.org/page/Main_Page
  [1]: http://1.bp.blogspot.com/-q0FBQlA8If8/T9sdfi5NqYI/AAAAAAAABYA/psQ0es2-Z-Q/s1600/2-enterprise-login.png
  [2]: http://2.bp.blogspot.com/-dcn3wCsPd-A/T9sdIijrqzI/AAAAAAAABXU/ch5I1l9JnRg/s1600/3-validate-domain.png
  [3]: http://2.bp.blogspot.com/-H91ZbCQ0WME/T9sdJDRfk7I/AAAAAAAABXc/cZwm-oCXMso/s1600/4-validate-login.png
  [4]: http://3.bp.blogspot.com/-D56zzIljSFY/T9sdJqkuxVI/AAAAAAAABXo/oY8D0VE-8AI/s1600/5-validate-password.png
  [don't require admin creds by default]: http://technet.microsoft.com/en-us/library/cc780195%28WS.10%29.aspx
  [5]: http://2.bp.blogspot.com/-D1mEGikSSaY/T9sdLbl0vXI/AAAAAAAABXw/g5h5AVD9cXo/s1600/6-administrator.png
  [6]: http://1.bp.blogspot.com/-48xy1QCK5i4/T9sdMemzYWI/AAAAAAAABX0/VReT8j4eqgk/s1600/7-added-accounts.png
  [realmd]: http://cgit.freedesktop.org/~stefw/realmd/
  [these guys figured out]: http://static.usenix.org/publications/compsystems/1996/win_davis.pdf
