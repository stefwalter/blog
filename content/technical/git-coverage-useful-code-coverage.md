Title: git-coverage: Useful code coverage
Date: 2012-12-18 10:55
Tags: technical, gnome
Slug: git-coverage-useful-code-coverage

I've sorta dabbled in using code coverage off and on, but it never
really grabbed me as super useful and fit well within my workflow.  
  
When hacking on open source I want to try out patches, run tests against
them, whether automatic unit tests or manually diddling things during
testing. What I'm really interested is whether I tested out all the bits
of the patch.  
  
Traditional coverage tools tell you about the coverage of the whole
project, which you then have to update and dig through to see if your
changes were covered. I really don't care about the code coverage of
entire projects. This is especially true if I'm just a contributor of a
few patches. I want to see the coverage of the stuff I just changed.  
  
![Git coverage](images/git-coverage-shot.png)
  
Anyhooo ... after much ongoing grumpiness ... I put together
[git-coverage][] which is a git plugin which you can use to look at a
patch and see which code paths have been run. Basically you use it
exactly like you would `git diff` but it highlights the code in the diff
that didn't have coverage. Works with Python, C and C++ code so far.  

<div class="separator" style="clear: both; text-align: center;">

  

</div>

To use with C or C++ code, you need to build the project using gcc's
`--coverage` info. Something like this:  
  
    :::text
    $ CFLAGS='--coverage' ./configure ...
    $ make clean all

  
Next you make some modifications to the code, rebuild, and run the tests
or code. Now the following command will tell you which of your changes
were covered.  
  
    :::text
    $ git coverage

  
Any lines that start with an exclam have no coverage. They're
highlighted in red if you have git coloring enabled. If there's no
output from the above command, then all lines have coverage. Similar to
how if nothing has changed, diff will output nothing.  
  
If you've already checked in your code then you would use the following
command to test the coverage of the last commit, similar to how you
might use git diff to show the last commit.  
  

    :::text
    $ git coverage HEAD~1

  
Or you can test coverage of the last N patches by doing stuff like:  
  

    :::text
    $ git coverage HEAD~3..

  
Normally `git-coverage` tries to check the coverage of lines surrounding
the changes. If you want to suppress this, you just pass in diff options
to narrow the diff down:  
  

    :::text
    $ git coverage --unified=0

  
To install `git-coverage` put it somewhere in your path. You might use:  
  
    :::text
    $ git clone git://thewalter.net/git-coverage
    $ cd git-coverage
    $ ln -s $PWD/git-coverage ~/.local/bin
  
To use with Python code, install the `python-coverage` package, and run
your code or tests like this:  
  

    :::text
    $ # Yup, python-coverage has a rather bold command name.
    $ coverage run /path/to/python-code args ...
    $ git coverage
  
Anyway, maybe this'll help someone. It's an itch I've had for some
time.  
  
BTW, [Phillip][] put some [code into gnome-common][] which you can use
to [add --enable-code-coverage to your configure script][], and
optionally get full coverage reports for the project. Obviously also
works with git-coverage.

  [git-coverage]: http://thewalter.net/git/cgit.cgi/git-coverage/
  [Phillip]: http://tecnocode.co.uk/
  [code into gnome-common]: http://git.gnome.org/browse/gnome-common/tree/macros2/gnome-code-coverage.m4
  [add --enable-code-coverage to your configure script]: http://git.gnome.org/browse/gcr/commit/?id=a185f4f20f20776f6b0dcccb4f3eeba30941022a
