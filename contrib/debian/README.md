
Debian
====================
This directory contains files used to package oakcoind/oakcoin-qt
for Debian-based Linux systems. If you compile oakcoind/oakcoin-qt yourself, there are some useful files here.

## oakcoin: URI support ##


oakcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install oakcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your oakcoin-qt binary to `/usr/bin`
and the `../../share/pixmaps/oakcoin128.png` to `/usr/share/pixmaps`

oakcoin-qt.protocol (KDE)

