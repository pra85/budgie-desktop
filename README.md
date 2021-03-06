budgie-desktop
==========

The Budgie Desktop is the flagship desktop of Solus OS.

NOTE:: The `master` branch is currently highly unstable, due to a rework.
For now use commit from before 0596e73177f74911eb93ccd6eca952c79b88f25f
or use the 10.2.2 tag, thanks!

License
-------

budgie-desktop is available under a split license model. This enables
developers to link against the libraries of budgie-desktop without
affecting their choice of license and distribution.

The shared libraries are available under the terms of the LGPL-2.1,
allowing developers to link against the API without any issue, and
to use all exposed APIs without affecting their project license.

The remainder of the project (i.e. installed binaries) is available
under the terms of the GPL 2.0 license. This is clarified in the headers
of each source file.

Building
--------

budgie-desktop has a number of build dependencies that must be present
before attempting configuration. The names are different depending on
distribution, so the pkg-config names, and the names within the Solus
Operating System, are given:

    - gobject-2.0 >= 2.44.0
    - gio-2.0 >= 2.44.0
    - gtk+-3.0 >= 3.16.0
    - gio-unix-2.0 >= 2.44.0
    - uuid
    - libpeas-gtk-1.0 >= 1.8.0
    - libgnome-menu-3.0 >= 3.10.1
    - gobject-introspection-1.0 >= 1.44.0
    - libpulse >= 2
    - mutter >= 3.18.0
    - gnome-desktop-3.0 >= 3.18.0
    - libwnck >= 3.14.0
    - upower-glib >= 0.9.20
    - polkit-agent-1 >= 0.110
    - polkit-gobject-1 >= 0.110

And:

    - vala >= 0.28

To install these on Solus:

```bash

    sudo eopkg it glib2-devel libgtk-3-devel libpeas-devel gobject-introspection-devel util-linux-devel pulseaudio-devel libgnome-menus-devel libgnome-desktop-devel mutter-devel polkit-devel libwnck-devel upower-devel vala
    sudo eopkg it -c system.devel
```

Clone the repository:

```bash

    git clone https://github.com/solus-project/budgie-desktop.git
```

Now build it:
```bash

    cd budgie-desktop
    ./autogen.sh --prefix=/usr
    make -j$(($(getconf _NPROCESSORS_ONLN)+1))
    sudo make install
```

Theming
------

Please look at `./data/theme/sass` to override aspects of the default
theming. Budgie theming is created using SASS, and the CSS files shipped
are minified. Check out `./data/theme/README.md` for more information
on regenerating the theme from SASS.

Alternatively, you may invoke the panel with the GTK Inspector to
analyse the structure::

```bash

    budgie-panel --gtk-debug=interactive --replace
```

If you are validating changes from a git clone, then::

```bash

    ./panel/budgie-panel --gtk-debug=interactive --replace
```

Note that for local changes, GSettings schemas and applets are expected
to be installed first with `make install`.

Note that it is intentional for the toplevel `BudgiePanel` object to
be transparent, as it contains the `BudgieMainPanel` and `BudgieShadowBlock`
within a singular window.

Also note that by default Budgie overrides all theming with the stylesheet,
and in future we'll also make it possible for you to set a custom theme.
To do this, test your changes in tree first. When you have a reasonable
theme put together, please open an issue and we'll enable setting of
a custom theme (no point until they exist.)

Testing
------

As and when new features are implemented - it can be helpful to reset
the configuration to the defaults to ensure everything is still working
ok. To reset the entire configuration tree, issue::

```bash

    dconf reset -f /com/solus-project/budgie-panel/  
```

Porting
------

If using a tarball, note that the distributed tarball includes the generated
.c and .h files. This means any Vala conditionals are invalidated. To
combat this, simply issue the following before doing any configure::

```bash

    make maintainer-clean
```

This ensures all the autogenerated `*.{c,h}` files are cleaned, and conditionals
will work with your build.

Known Issues
-----------

Currently the GtkPopover can *randomly* glitch when the panel is at the
bottom of the screen. It is expected to be fixed in a later commit, however
let's be fair, it does kinda look better up top.

*Update*: This only happens on the *first* show of the applet.

Authors
=======

Copyright (C) 2015 Ikey Doherty <ikey@solus-project.com>
