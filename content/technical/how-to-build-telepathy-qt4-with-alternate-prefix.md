Title: How to build telepathy-qt4 with alternate prefix
Date: 2011-08-11
Tags: technical
Slug: how-to-build-telepathy-qt4-with

Just figured out how to build telepathy-qt4 in an alternate prefix and
also look for dependencies in that prefix as well. Since I don't use
cmake much these days, figured I'd post this so I could go and look back
at it later. Depends on [this fix][].  
  
    :::sh
    PKG_CONFIG_PATH=~/the/prefix cmake -DCMAKE_INSTALL_PREFIX=~/the/prefix  .make install

Or if on a 64-bit system:

    :::sh
    PKG_CONFIG_PATH=~/the/prefix cmake -DCMAKE_INSTALL_PREFIX=/data/build/telepathy -DLIB_SUFFIX=64 .make install

  [this fix]: https://bugs.freedesktop.org/show_bug.cgi?id=40008
