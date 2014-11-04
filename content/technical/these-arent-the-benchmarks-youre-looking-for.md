Title: These aren't the benchmarks you're looking for
Date: 2010-10-19
Tags: technical, gnome
Slug: this-arent-benchmarks-youre-looking-for

  
I was evaluating use of [GObject][] for small plentiful
short-lived objects in [libgck][]. I wanted to see how their performance
compared to custom reference counted structures. Turns out it's not as
bad as I imagined.   
  
The speed difference on my system, with a [simple test program][], ended
up being around a factor of eight. Most of that cost is due to
`pthread_mutex_lock` and `pthread_mutex_unlock`.  

    :::text
    $ ./test-gobject-speed
    struct x 10000000 = 1.091700
    object x 10000000 = 8.578848

Note that I didn't use heavy weight stuff like properties or signals in
my benchmark. But I don't need those for this use case.


  [GObject]: http://library.gnome.org/devel/gobject/unstable/
  [libgck]: http://stef.thewalter.net/2010/10/introducing-libgck-pkcs11-gobject.html
  [simple test program]: http://thewalter.net/stef/misc/test-gobject-speed.c
