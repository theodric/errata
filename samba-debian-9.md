Something is fucky with Samba in Debian 9 as of 2017-07-18. I recommend holding Samba+depends before doing a dist-upgrade.

echo samba hold | dpkg --set-selections
echo samba-common hold | dpkg --set-selections
echo samba-common-bin hold | dpkg --set-selections
echo samba-dsdb-modules hold | dpkg --set-selections
echo samba-libs hold | dpkg --set-selections
echo samba-vfs-modules hold | dpkg --set-selections
