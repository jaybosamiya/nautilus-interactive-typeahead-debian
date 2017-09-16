# Interactive Typeahead for Nautilus on Debian

At some point of time, the Nautilus developers decided to drop the idea of interactive typeahead - the feature that allows you to type in the start of a file and have it be _selected_ in the current directory, instead of applying the typed text as a _filter_ on a _recursive_ search.

This project aims to bring back that feature, by patching the source code back to what it should be.

## Shoutout

A big shoutout to ElectricPrism who has a set of patches on at his [repository](https://github.com/ElectricPrism/nautilus-typeahead-desktop-buff-aur) for AUR, which I adapted to Nautilus 3.22.3-1 on Debian.

## Warning

This project has been only tested on Debian Stable (Stretch) with Nautilus 3.22.3-1

If you are not on this exact version, YMMV.

## Quick how-to install

A set of `.deb` files are in this project's releases (and also in this folder). Simply download that to a new directory, and run `sudo dpkg -i *.deb` to install it, and then run `gsettings set org.gnome.nautilus.preferences enable-interactive-search true` to enable interactive typeahead. You might need to run `nautilus -q` once for the changes to be incorporated (or just reboot).

## How to remove

It should be as easy as `sudo apt install --reinstall nautilus`, but why would you want to remove this feature?

The recursive search is still accessible via `Ctrl+F`, and if you want to disable interactive typeahead, simply run `gsettings set org.gnome.nautilus.preferences enable-interactive-search false`.

## How to build?

First off, we will need to have the Debian build tools, as well as the dependencies for building Nautilus:

```
sudo apt install devscripts
sudo apt build-dep nautilus
```

Then, make a new folder to build nautilus:

```
mkdir nautilus-build
cd nautilus-build
```

Download nautilus, and apply our patch:

```
apt source nautilus
cd nautilus-3.22.3
patch -p1 -i ../../nautilus-restore-typeahead.patch
```

Build nautilus:

```
fakeroot debian/rules binary
```

This will generate a bunch of `.deb` files which we then install:

```
sudo dpkg -i ../*.deb
```

## License

```
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <https://unlicense.org>
```
