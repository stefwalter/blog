Title: How to create an Active Directory domain to test against
Date: 2012-08-03
Tags: technical, security
Slug: how-to-create-active-directory-domain

Many interested people want to help test the Active Directory work and
bug fixes we've been doing. But sadly there's no public Active Directory
servers that I know of. So here's how to setup a virtual machine with
your own Active Directory. It's not that hard.  
  

### 1. Preparation

-   Each Active Directory has a unique domain name. Choose one. You can
    choose a subdomain of a domain you own, or one that's completely
    made up. I chose `borg.thewalter.lan`
-   Download the evaluation edition of [Windows 2008 R2 Enterprise server][]. Click the *Get Started *button at that link to download
    it. The evaluation edition is valid for 180 days. You should end up
    with an ISO file named something
    like: `7601.17514.101119-1850_x64fre_server_eval_en-us-GRMSXEVAL_EN_DVD.iso`
-   We'll be using virt-manager in this tutorial, so install
    `virt-manager`, `libvirtd, qemu` and all their dependencies.

### 2. Create a virtual network

-   The Active Directory server will need a static IP address. The
    `default` virtual network configured by libvirtd does not have any
    space for a static IP address, so we need to create a new virtual
    network.
-   Start `virt-manager` and make sure you're connected to the
    *localhost (QEMU)* connection.
-   Click on *localhost (QEMU)* and choose *Edit* \> *Connection
    Details* from the menu.
-   Choose the *Virtual Networks* tab in the dialog that pops up and
    click the add button.
-   Use settings like:  
   *Network Name*: ad  
   *Network*: `192.168.12.0/24`  
   *Enable DHCP*: checked  
   *Start*: `192.168.12.128`  
   *End*: `192.168.12.254`
-   Notice that we left some space between the start of the netblock and
    the first DHCP allocated address. Actually virt-manager does this by
    default for added virtual networks like this one.
-   You probably want to *Forward* (via *NAT*) to your physical network.
    That makes it easier to activate windows later and get updates.
-   Complete the wizard and you're done.

### 3. Create a new virtual machine

-   In the main virt-manager window, click the create button in the
    toolbar to create a new virtual machine.
-   Type the domain name you chose above as the virtual machine *Name*.
-   Choose *Local install media* and when prompted select the *ISO
    image* you downloaded above as the *install media*.
-   Set *OS type* to *Windows*, and *Version* to *Microsoft Windows
    Server 2008.*
-   512 MB of memory is enough, 1 CPU is enough. Feel free to set these
    higher if you like.
-   Create a new virtual disk with at least 10 GB of disk space.
-   On the last page of the *Create a new virtual machine* dialog,
    expand the *Advanced options* section and choose the network you
    created above.
-   Complete the dialog and the virtual machine should be created. Then
    the Windows install should begin.

### 4. Windows Server install

-   Choose whatever keyboard and language you want on the first dialog
    of the install.
-   On the next page choose *Install now*.
-   A list of types of Windows Server installs should show up. Choose
    *Windows Server 2008 R2 Standard (Full Installation)*and go to the
    next page.
-   Read and accept the license.
-   Choose *Custom (advanced)* when prompted how to install Windows.
-   Select the disk to install Windows on. There should only be one
    choice which is the virtual disk you configured when you created the
    virtual machine.
-   Windows Server will proceed to install, and will reboot a couple
    times in the process.
-   Once the system is ready, you will be prompted to change the
    *Administrator* password. This is actually setting the password for
    the first time. This is the password for the *Administrator* account
    on the server itself, not the administrator of the Active Directory
    domain, which you'll set later. You can use the same password for
    both, since this is a test install.
-   If all goes well you should be logged into your new server at this
    point. A bunch of helpful windows will pop up, but you don't need to
    do anything with them.

### 5. Set the IP address

-   An Active Directory server acts as an LDAP and DNS server, and needs
    a static IP address.
-   Click *Start* \> *Network,*and then click the *Change adapter
    settings* link in the window that comes up. Another window should
    appear.
-   Right click on the *Local Area Connection* item and choose
    *Properties* in the menu.
-   Click on the *Internet Protocol Version 4 (TCP/IPv4)* item and then
    click the *Properties* button. A dialog for setting the addresses
    comes up.
-   Choose *Use the following IP address.*Then set the relevant fields.
    The settings here are based on the virtual network you created
    above, if you used a different netblock then modify as appropriate:  
   *IP Address*: `192.168.12.10`  
   *Subnet mask*: `255.255.255.0`  
   *Default gateway*: `192.168.12.1`  
   *Preferred DNS Server*: `192.168.12.1`
-   Click OK or Close in the various dialogs to complete things.

### 6. Set the machine name

-   An Active Directory server should have a well known DNS name, you
    don't need to set it in DNS, but just name the server appropriately
    and then Active Directory will do the rest.
-   Click *Start* \> *Computer*, and a window should come up.
-   In the left pane of the window, there's an item called
    *Computer.*Right click on it and choose *Properties* from the menu.
    Another window should show up.
-   Click *Change Settings*, and a dialog will come up.
-   In the *Computer Name* tab click the *Change...* button, which
    displays another dialog.
-   Set `DC` as the *Computer name* or another name of your choice.
    Don't worry about the *Member of Domain or Workgroup* stuff yet.
-   Click OK and/or Close to complete the changes. You'll be prompted to
    restart, so go ahead and do that.

### 7. Setting up Active Directory

-   Click *Start \>* *Run* and type `DCPROMO` in the dialog that comes
    up.
-   A progress window will come up which explains about installing some
    components. This takes a while.
-   A wizard will eventually show up. Click through the introduction and
    warnings.
-   Choose *Create a new domain in a new forest*.
-   On the next page enter the domain you chose earlier, like
    `borg.thewalter.lan`
-   Choose the *Forest functional level*. You can choose whichever one
    you like. Choosing *2008 R2* is a decent choice. You can test
    against various Active Directory servers with different levels to
    simulate different domains you might encounter in the wild.
-   Make sure *DNS Server* is chosen on the next page.
-   Once you complete that, a dialog will come up warning you about how
    the DNS delegation cannot be created. We'll do that manually below,
    so this is nothing to worry about. Choose *Yes*.
-   Leave the default paths for database and log files.
-   Choose a domain *Administrator* password. Logically this is
    different from the local server *Administrator* account you set the
    password for above. But you can use the same password to keep things
    simple.
-   Review the selections if you're interested, and then click *Next* to
    complete things.
-   Wait for a while for installation and configuration, *Finish. *
-   You'll need to *Restart Now*.
-   The reboot after installing Active Directory will take a while as it
    does a bunch of stuff on the first boot.

### 8. Setup DNS to work with Active Directory

-   Back on your linux box you'll want to be able to connect to Active
    Directory. To do this you need to setup DNS. Active Directory comes
    with its own DNS server, you just need to tell your local host where
    it is. To do this we'll install a local caching name server.
-   Install bind. If you're on Fedora you can use a command
    like: `# yum install caching-nameserver`
-   After the install completes, edit `/etc/named.conf` and add the
    following line to your main *options* section:  

        :::text
        forwarders { 8.8.8.8; /* ... or the address of your ISP DNS server */ };

-   And add this to the end of `/etc/named.conf`. Modify for your domain
    name or server static IP address assigned above:  

        :::text
        zone "borg.thewalter.lan" { type stub; masters { 192.168.12.10; }; };

-   Restart the named service with: `# systemctl restart named.service`
-   Before configuring your host to use the local caching nameserver,
    test it with commands like:   

        :::text
        # host borg.thewalter.lan 127.0.0.1
        # host dc.borg.thewalter.lan 127.0.0.1
        # host google.com 127.0.0.1

-   Once you know it's working, use `nm-connection-editor` to edit your
    connection. Choose your connection, and on the *IPv4 Settings* tab,
    choose *Automatic (DHCP) addresses only.*Now set `127.0.0.1` as the
    *DNS servers*.
-   You should now be able to test you local server with commands like:  

        :::text
        # host borg.thewalter.lan
        # host dc.borg.thewalter.lan
        # host google.com

### 9. Test the Active Directory domain works

-   On your host linux box you should now be able to get a kerberos
    ticket.
-   If you have a custom configured `/etc/krb5.conf`, you may need to
    remove or move it. There is no real need for this file with a modern
    kerberos domain like Active Directory.
-   Run this command. Make sure the domain is upper case here:

        :::text
        $ kinit Administrator@BORG.THEWALTER.LAN

-   You'll be prompted for the domain *Administrator* password. The one
    you typed in the *Setting up Active Directory* step above.
-   If successful `kinit` will show no output. You can use the `klist`
    command to see your ticket.

That's it. You're done. 

You can add additional Active Directory users via the *Active Directory
Users and Computers* tool in the *Administrative Tools* section of the
*Start* menu in the Windows Server virtual machine.

You may be prompted to Activate your Windows install. You won't need any
special information or keys or anything. Just go ahead with it. The
install you have is valid for 180 days, and will say in the lower left
corner how long you have left.

  [Windows 2008 R2 Enterprise server]: http://technet.microsoft.com/en-us/evalcenter/dd459137.aspx
