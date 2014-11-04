Title: Introspecting Certificates
Date: 2011-09-29
Tags: technical, security, gnome
Slug: introspecting-certificates

Today I merged in a contribution from Evan Nemerson for GObject
introspection support into the Gcr and Gck libraries. I ended up
tweaking thousands of lines of comments and code,
[filed][] [some][] [bugs][] and so forth.  
  
But the end result is you use PKCS\#11 and stuff like the [Gcr
certificate widgets][], from languages like python and javascript
(although not your browser). For example this:  
  

    :::javascript
    const Gck = imports.gi.Gck;
    const Gcr = imports.gi.Gcr;
    const Gtk = imports.gi.Gtk;

    /* TODO: From pkcs11.h */
    const CKA_CLASS = 0;
    const CKO_CERTIFICATE = 1;
    const CKA_VALUE = 17;
    const URI = "pkcs11:object-type=cert";

    Gtk.init(null, null);
    var dialog = new Gtk.Dialog();

    var viewer = new Gcr.ViewerWidget();
    dialog.get_content_area().pack_start(viewer, true, true, 0);

    var modules = Gck.modules_initialize_registered(null);
    var objects = Gck.modules_objects_for_uri(modules, URI, Gck.SessionOptions.READ_ONLY);

    objects.forEach(function(object) {
        viewer.load_data(null, object.get_data(CKA_VALUE, null));
    });

    viewer.show();
    dialog.run();

  
... will pop up a window show up with every certificate on every smart
card and key storage [you have configured][]. All of this goodness is in
[gnome-keyring git master][].

  [filed]: https://bugzilla.gnome.org/show_bug.cgi?id=660436
  [some]: https://bugzilla.gnome.org/show_bug.cgi?id=581525
  [bugs]: https://bugzilla.gnome.org/show_bug.cgi?id=660352
  [Gcr certificate widgets]: http://developer.gnome.org/gcr/unstable/gcr-GcrCertificateWidget.html
  [you have configured]: http://p11-glue.freedesktop.org/doc/p11-kit/config-example.html
  [gnome-keyring git master]: http://git.gnome.org/browse/gnome-keyring/?h=master
