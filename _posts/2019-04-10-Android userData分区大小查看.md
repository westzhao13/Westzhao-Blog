# Android userData分区大小查看

首先，可以用个命令 cat /proc/partitions 查看分区大小如下所示：（单位KBytes）

cat /proc/partitions

major minor  #blocks  name

 179        0   15388672 mmcblk0
 179        1       1024 mmcblk0p1
 179        2       1024 mmcblk0p2
 179        3      10240 mmcblk0p3
 179        4       2048 mmcblk0p4
 179        5       8192 mmcblk0p5
 179        6       8192 mmcblk0p6
 179        7      20480 mmcblk0p7
 259        0      20480 mmcblk0p8
 259        1      40960 mmcblk0p9
 259        2      40960 mmcblk0p10
 259        3      40960 mmcblk0p11
 259        4      20480 mmcblk0p12
 259        5       1024 mmcblk0p13
 259        6     307200 mmcblk0p14
 259        7      40960 mmcblk0p15
 259        8     819200 mmcblk0p16
 259        9     819200 mmcblk0p17
 259       10   13186048 mmcblk0p18
 179       16       4096 mmcblk0boot1
 179        8       4096 mmcblk0boot0

第一个mmcblk0为emmc的块设备，大小为15388672 KB。

其他的分区我们可以通过命令 

ls -l /dev/block/platform/soc/by-name

lrwxrwxrwx root     root              2019-03-13 16:38 baseparam -> /dev/block/mmcblk0p5
lrwxrwxrwx root     root              2019-03-13 16:38 bootargs -> /dev/block/mmcblk0p2
lrwxrwxrwx root     root              2019-03-13 16:38 cache -> /dev/block/mmcblk0p17
lrwxrwxrwx root     root              2019-03-13 16:38 deviceinfo -> /dev/block/mmcblk0p4
lrwxrwxrwx root     root              2019-03-13 16:38 fastboot -> /dev/block/mmcblk0p1
lrwxrwxrwx root     root              2019-03-13 16:38 fastplay -> /dev/block/mmcblk0p9
lrwxrwxrwx root     root              2019-03-13 16:38 fastplaybak -> /dev/block/mmcblk0p10
lrwxrwxrwx root     root              2019-03-13 16:38 kernel -> /dev/block/mmcblk0p11
lrwxrwxrwx root     root              2019-03-13 16:38 logo -> /dev/block/mmcblk0p7
lrwxrwxrwx root     root              2019-03-13 16:38 logobak -> /dev/block/mmcblk0p8
lrwxrwxrwx root     root              2019-03-13 16:38 misc -> /dev/block/mmcblk0p12
lrwxrwxrwx root     root              2019-03-13 16:38 pqparam -> /dev/block/mmcblk0p6
lrwxrwxrwx root     root              2019-03-13 16:38 qbboot -> /dev/block/mmcblk0p13
lrwxrwxrwx root     root              2019-03-13 16:38 qbdata -> /dev/block/mmcblk0p14
lrwxrwxrwx root     root              2019-03-13 16:38 recovery -> /dev/block/mmcblk0p3
lrwxrwxrwx root     root              2019-03-13 16:38 system -> /dev/block/mmcblk0p16
lrwxrwxrwx root     root              2019-03-13 16:38 trustedcore -> /dev/block/mmcblk0p15
lrwxrwxrwx root     root              2019-03-13 16:38 userdata -> /dev/block/mmcblk0p18

我们可以找到 userdata -> /dev/block/mmcblk0p18 userdata分区被链接到了mmcblk0p18这个分区，再对应到第一条命令中我们可以查找到13186048 KB

在android中，device里面的BroadConfig.mk中可以修改userdata的大小，注意这里面的单位为字节

```makefile
TARGET_USERIMAGES_USE_EXT4 := true
BOARD_SYSTEMIMAGE_PARTITION_SIZE := 838860800
BOARD_USERDATAIMAGE_PARTITION_SIZE := 13502513152
BOARD_CACHEIMAGE_PARTITION_SIZE := 838860800
BOARD_CACHEIMAGE_FILE_SYSTEM_TYPE := ext4
BOARD_FLASH_BLOCK_SIZE := 4096
BOARD_HAVE_BLUETOOTH := true
```

