archlinux-orientdb-community
============================

Arch Linux package for OrientDB Community Edition verion 2.1.3 GA (as of 4th Oct 2015)

DESCRIPTION:
For this version of "archlinux-orientdb-community", the released tar.gz is used, and thus the build of the java sources is not required.
File downloaded is, http://orientdb.com/download.php?email=unknown@unknown.com&file=orientdb-community-2.1.3.tar.gz

INSTRUCTIONS:
Download this source to a convenient user directory.

makepkg or makepkg -f if repeating.

updpkgsums if there is an integrity error, then makepkg again.

sudo pacman -U 'xz file' where the xz file is that generated by makefile.

sudo systemctl start orientdb

sudo systemctl enable orientdb

October 2015
John Radley (jradxl [at] gmail [dot] com)

