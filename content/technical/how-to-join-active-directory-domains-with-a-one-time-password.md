Title: How to join Active Directory domains with a One Time Password
Date: 2014-05-06 14:23
Tags: active-directory
Slug: how-to-join-active-directory-domains

[realmd][] and [adcli][] allow you to join a domain with a one time
password.  
  
That is: a domain administrator can prepare a one time password, and
that one time password can later be used (usually by someone else) to
join a specific computer to the domain.  
  
[FreeIPA][] supports this natively. But adcli also accomplishes this for
Active Directory domains. People have been asking how that happens.  
  
Each computer in an Active Directory domain has a computer account. Each
computer account has a computer password. Normally this password is
[randomly generated][] while joining the domain.  
  
When you choose the *Reset Password* option in the Active Directory UI,
this password is set to a predictable string, which is just the computer
account name in lower case (ie: `samAccountName` without the dollar
sign).  
  

![Reset computer](images/reset-computer.png)

  
Since computer accounts can (by default) change their own account
passwords, reseting a computer account allows anyone to claim the
computer account, by changing its password from this known password to a
generated one.  
  
realmd takes advantage of the above, and will automatically join a
domain if the relevant computer account has been reset.  
  
In addition adcli has a `preset-computer` mode which allows an
administrator to generate a new computer account, and set its paswsord
to a one time use password.  
  

    :::text
    $ adcli preset-computer --domain=ad.example.com --one-time-password=ThisIsthe1xPass computer1.example.com
    Password for Administrator@AD.EXAMPLE.COM:
    computer-name: COMPUTER1

  
This one time password can later be used with realmd to have it join the
computer account, like so:  
  
    :::text
    $ hostname
    computer1.example.com
    $ realm join --one-time-password=ThisIsthe1xPass ad.example.com

  
Or you can use this one time password with kickstart, as shown here:  
  
  
<iframe allowfullscreen="" src="//www.youtube.com/embed/1Tm1jZ8fpW4" frameborder="0" height="720" width="960"></iframe>

  [realmd]: http://www.freedesktop.org/software/realmd/docs/
  [adcli]: http://www.freedesktop.org/software/realmd/adcli/adcli.html
  [FreeIPA]: http://www.freeipa.org/page/Main_Page
  [randomly generated]: http://cgit.freedesktop.org/realmd/adcli/tree/library/adenroll.c#n185
