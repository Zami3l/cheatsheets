Add partition
===

### 0. Versions
FreeBSD : **13.0**  
Type : **UEFI**

### 1. Show current partition
```shell
$ gpart show
freebsd# gpart show -l
=>        40  1048575920  da0  GPT  (500G)
          40        1024    1  (null)  (512K)
        1064     8388608    2  swapfs  (4.0G)
     8389672   314572800    3  rootfs  (150G)
   322962472   725613488       - free -  (346G)
```

### 2. Add new partition
```shell
$ gpart add -t freebsd-ufs -l datafs -s 100G da0
da0p4 added

$ gpart show -l
=>        40  1048575920  da0  GPT  (500G)
          40        1024    1  (null)  (512K)
        1064     8388608    2  swapfs  (4.0G)
     8389672   314572800    3  rootfs  (150G)
   322962472   209715200    4  datafs  (100G)
   532677672   515898288       - free -  (246G)
```

### 3. Format the new filesystem
```shell
$ newfs -U /dev/da0p4
/dev/da0p4: 102400.0MB (209715200 sectors) block size 32768, fragment size 4096
        using 164 cylinder groups of 625.22MB, 20007 blks, 80128 inodes.
        with soft updates
super-block backups (for fsck_ffs -b #) at:
 192, 1280640, 2561088, 3841536, 5121984, 6402432, 7682880, 8963328, 10243776, 11524224, 12804672, 14085120, 15365568,
 16646016, 17926464, 19206912, 20487360, 21767808, 23048256, 24328704, 25609152, 26889600, 28170048, 29450496,
 30730944, 32011392, 33291840, 34572288, 35852736, 37133184, 38413632, 39694080, 40974528, 42254976, 43535424,
 44815872, 46096320, 47376768, 48657216, 49937664, 51218112, 52498560, 53779008, 55059456, 56339904, 57620352,
 58900800, 60181248, 61461696, 62742144, 64022592, 65303040, 66583488, 67863936, 69144384, 70424832, 71705280,
 72985728, 74266176, 75546624, 76827072, 78107520, 79387968, 80668416, 81948864, 83229312, 84509760, 85790208,
 87070656, 88351104, 89631552, 90912000, 92192448, 93472896, 94753344, 96033792, 97314240, 98594688, 99875136,
 101155584, 102436032, 103716480, 104996928, 106277376, 107557824, 108838272, 110118720, 111399168, 112679616,
 113960064, 115240512, 116520960, 117801408, 119081856, 120362304, 121642752, 122923200, 124203648, 125484096,
 126764544, 128044992, 129325440, 130605888, 131886336, 133166784, 134447232, 135727680, 137008128, 138288576,
 139569024, 140849472, 142129920, 143410368, 144690816, 145971264, 147251712, 148532160, 149812608, 151093056,
 152373504, 153653952, 154934400, 156214848, 157495296, 158775744, 160056192, 161336640, 162617088, 163897536,
 165177984, 166458432, 167738880, 169019328, 170299776, 171580224, 172860672, 174141120, 175421568, 176702016,
 177982464, 179262912, 180543360, 181823808, 183104256, 184384704, 185665152, 186945600, 188226048, 189506496,
 190786944, 192067392, 193347840, 194628288, 195908736, 197189184, 198469632, 199750080, 201030528, 202310976,
 203591424, 204871872, 206152320, 207432768, 208713216
```

### 4. Add new partition on fstab
```shell
$ vim /etc/fstab
# Device        Mountpoint      FStype  Options Dump    Pass#
/dev/da0p2      none            swap    sw      0       0
/dev/da0p3      /               ufs     rw      1       1
/dev/da0p4      /data           ufs     rw      2       2
```