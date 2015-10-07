# images-catchwire-kalitap

Legend:
-------
SD images for CatchWire, KaliTAP, nTAP and other WAW appliances.


Credentials:
------------
Root access:	root:root

User Access:	ubuntu:temppwd


Configuration:
--------------
/etc/rc.local - rpcapd, nprobe, udp0srv and other stuff.


Partitions:
-----------
catchwire-XXXXXXXX.img fstab partitions (relatively power failure safe):

 >> /dev/mmcblk0p1 /boot vfat ro                                 0 2

 >> /dev/mmcblk0p2 /     ext4 defaults,errors=remount-ro,noatime 0 1

catchwire-ro-XXXXXXXX.img fstab partitions (total power failure safe - EXPERIMENTAL):

 >> proc           /proc    proc   defaults                           0 0

 >> /dev/mmcblk0p1 /boot    vfat   ro,noatime                         0 2

 >> /dev/mmcblk0p2 /        ext4   ro,noatime                         0 1

 >> /dev/mmcblk0p3 /home    ext4   defaults,errors=remount-ro,noatime 0 2

 >> none           /var/log tmpfs  size=1M,noatime                    0 0

 >> none           /var/tmp tmpfs  size=1M,noatime                    0 0

 >> none           /tmp     tmpfs  size=1M,noatime                    0 0


In order to make partition RW one can use the following commands:
(under root priviledges)

 >> mount -o remount,rw /dev/mmcblk0p1

 >> mount -o remount,rw /dev/mmcblk0p2

 >> mount -o remount,rw /dev/mmcblk0p3


Images:
-------
(Inspired by www.shayanderson.com)

This example will show how to GZIP a large file and split the file into smaller multiple files. First, GZIP the large file:

 >> gzip -c newlarge.img > newlarge.img.gz

Next, use the split command to split the files into multiple smaller files:

 >> split -b 100m "newlarge.img.gz" "newlarge.img.gz.part-"

This command will create smaller files, with the prefix "newlarge.img.gz.part-". So using this example, if we spit a 200mb file the example would output these files: 

 >> newlarge.img.gz.part-aa

 >> newlarge.img.gz.part-ab

Finally, when you want to join the split files and extract the GZIP large file use this command: 

 >> cat newlarge* > newlarge.img.gz

OR you can name each file individually: 

 >> cat newlarge.img.gz.part-aa newlarge.img.gz.part-ab > newlarge.img.gz

Now the smaller files should be combined to build the "newlarge.img.gz" file, which can now be extracted:

 >> gunzip newlarge.img.gz

