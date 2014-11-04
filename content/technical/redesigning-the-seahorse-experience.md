Title: Redesigning the Seahorse Experience
Date: 2011-10-17
Tags: technical, security, gnome
Slug: redesigning-seahorse-experience

As part of the work on getting smart cards into Seahorse, there's some
design work that needs to be done to make the new functionality usable.
In particular, the overarching design goal is that Seahorse isn't a tool
we expect users to "learn". Actions should follow mostly from the
passwords and keys that have been accumulated.  
  
So I've been working on the experience a bit. Some concepts:  
  

-   When most user's arrive, they should see their personal passwords,
    and keys or certificates if they have any listed. In this mode we
    combine items from all the various places these things are stored.
-   The user sees a certificate regardless of it's on a smart card,
    Gnome Keyring, or in NSS's store.
-   Each item should have an icon, and text describing what it is.
-   By default only 'personal' passwords and keys are shown. Those
    belonging to the user. So things like Trusted Root CA's don't litter
    the combined listing. This is easily changed on the 'View' menu.
-   The list is easily filterable by typing in the box.
-   We make sure to unlock the default password keyring when seahorse is
    started. Normally it's unlocked already, but just in case.

A screenshot (the toolbar needs some work): 

![Seahorse combined view](images/seahorse-combined-view.png)

So the experience starts off really straight forward, no need to clutter
things with where these items are coming from. If the user has a smart
card inserted, the certificates and keys on the smart card will also
show up there.

In order to see and manage stuff related to where the keys come from,
the user chooses 'View | Places' from the menu. A sidebar appears, which
supports the following concepts:  
  

-   Click on a place to view items from a that 'place'.
-   See which keyrings exist, delete, change master passwords etc.
-   See smart cards that are inserted.

  
A screenshot (the places need some tweaking):  
  

![Screenshot with places](images/screenshot-with-places2.png)

Something I've also been playing with is an easy to use multiple
selection. For example I'd like the user to be able to select multiple
places (let's say all the password keyrings), and see their items
together.  
  
I wanted to do something where check boxes are shown to the right of
each 'place' when the Alt-key is depressed. The user then would click
those checkboxes to select multiple places, and show their items
together. Once one box is checked, all check boxes remain visible. This
fits in with the concept of showing keyboard mnemonics when Alt is
pressed, and also GNOME seems to be using a
show-advanced-shortcuts-on-Alt-key concept here and there, and I thought
this would fit nicely. However, sadly the window manager grabs the mouse
when Alt is held down, for the purpose of full window drags, so I had to
think of something else.  
  
What I came up with was that a check box is shown next to a place when
that place is selected and focused. If the user clicks that check box,
then all the check boxes next to the other places become visible, and
more than one can be selected. As long as one is checked, all the check
boxes are visible. Works well enough, and should work with touch devices
as a bonus. But I'm not as satisfied as I would have been with the Alt
concept.  
  
Of course this is an advanced feature, and not necessarily something
that needs to be super 'beautiful' but none the less it was interesting
to try out these alternatives.  
  
There's lots more [design][] [work][] that needs to be done. For
example, I'd also like to integrate the new control center style
'Unlock' button in a way that makes sense. It gets complicated because
there's more than one thing to unlock (ie: smart cards, password
keyrings, etc.)  
  
Most of this is done in such a way that the pieces can be reused
elsewhere in other apps as well. Available right now in the seahorse
[refactor branch][] and depends on an up to date [build of the Gcr
library][]. Hopefully I'll be merging this into seahorse master soon.  
  
Oh, and thanks to [NLnet][] for sponsoring [Collabora][] to work on the
Seahorse smart card support.

</p>

  []: http://3.bp.blogspot.com/-utsxZuycKuQ/TpxZ9iNZ5oI/AAAAAAAAAjI/wM5Fxxv0FQ0/s1600/seahorse-combined-view.png
  [1]: http://1.bp.blogspot.com/-zY7nRuOwnx0/TpxbaKD0MKI/AAAAAAAAAjY/nwlTKih3DYw/s1600/screenshot-with-places2.png
  [design]: https://bugzilla.gnome.org/show_bug.cgi?id=656956
  [work]: https://bugzilla.gnome.org/show_bug.cgi?id=644214
  [refactor branch]: http://git.gnome.org/browse/seahorse/log/?h=refactor
  [build of the Gcr library]: http://git.gnome.org/browse/gcr/log/
  [NLnet]: http://nlnet.nl/
  [Collabora]: http://www.collabora.com/
