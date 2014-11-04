Title: Certificate and Key Widgets
Date: 2010-10-08
Tags: technical, security, gnome
Slug: certificate-and-key-widgets

The new certificate and key view widgets are now merged into
gnome-keyring master. They live in [libgcr][]: a library for crypto UI
widgets and crypto helpers.  
  
The goal of the widgets are to have a simple mode, where only the
information needed for a user to uniquely identify a certificate is
displayed. The widget can be expanded to show all the details about the
certificate.  
  
![Certificate](images/certificate-1.png)

Simple mode, with a dialog border.


![Certificate](images/certificate-2.png)
  


Details expanded.

At GUADEC [Matthew Paul Thomas][] helped us design a nice certificate
selector, and I'm working on implementing that.


  [libgcr]: http://git.gnome.org/browse/gnome-keyring/tree/gcr
  [Matthew Paul Thomas]: http://mpt.net.nz/
