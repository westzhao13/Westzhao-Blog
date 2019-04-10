# Android中获取rom的容量容量读取

```java
  File path = Environment.getExternalStorageDirectory();
  StatFs sf = new StatFs(path.getPath());
  long blocksize = sf.getBlockSizeLong();
  long totalblocks = sf.getBlockCountLong();
  //return Formatter.formatFileSize(getActivity(), blocksize * totalblocks);
  return (blocksize * totalblocks) / 1024 / 1024 + "MB";
```

这里主要是用到了Environment类，获取外部存储的容量如下代码所示，获取的是/storage/sdcard 的容量

```java
Environment.getExternalStorageDirectory()
```

可以用busybox df -h命令查看，分区的大小如下：

busybox df -h

Filesystem                Size      Used Available Use% Mounted on
tmpfs                   464.5M     56.0K    464.4M   0% /dev
tmpfs                   464.5M         0    464.5M   0% /mnt
tmpfs                   464.5M         0    464.5M   0% /mnt/secure
tmpfs                   464.5M         0    464.5M   0% /mnt/asec
tmpfs                   464.5M         0    464.5M   0% /mnt/obb
/dev/block/platform/soc/by-name/system   774.9M    592.1M    166.8M  78% /system

/dev/block/platform/soc/by-name/userdata 12.3G    403.2M     11.8G   3% /data

/dev/block/platform/soc/by-name/cache     774.9M    908.0K    758.0M   0% /cache

/data/media              12.2G    419.2M     11.7G   3% /storage/emulated
/data/media              12.2G    419.2M     11.7G   3% /storage/sdcard

如上所示，系统以及将/data/media挂载到了/stroage/sdcard的目录上。

利用如下的方法可以进行获取对应目录中的容量大小：

```java
Environment.getExternalStoragePublicDirectory(DIRECTORY_XXX) //DIRECTORY有ALARMS DCIM MOVIES等等用于获取sdcard中的对应目录
Environment.getDataDirectory() //用于获取 /data 目录下的空间大小
Environment.getDownloadCacheDirectory() //用于获取 /cache 目录下的大小
Environment.getRootDirectory() //用于获取 /system 目录下的大小
```



