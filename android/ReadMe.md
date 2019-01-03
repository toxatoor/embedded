# Platform-tools

Latest platform-tools

[Linux](https://dl.google.com/android/repository/platform-tools-latest-linux.zip) |
[Mac](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip) |
[Windows](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) 

# Busybox official builds

[https://busybox.net/downloads/binaries/](https://busybox.net/downloads/binaries/)

# Static binaries 

[aarch64-binaries.tar.gz](aarch64-binaries.tar.gz) - a set of statically build binaries including: 

- busybox 
- dropbear 
- strace 

Could be used for extending non-rooted phones. 

Non-rooted phones don't allow to execute binaries from /sdcard and any other available location under the visble part of FS tree. However, most of firmwares have a hidden directory allowing to set executable bit: /data/local/tmp. 
So any binary could be placed there via adb: 
```
$ adb push busybox /data/local/tmp
$ adb shell
HWINE:/ $ cd /data/local/tmp
HWINE:/data/local/tmp $ ./busybox
BusyBox v1.22.1 (2014-11-06 17:00:22 EST) multi-call binary.
BusyBox is copyrighted by many authors between 1998-2012.
Licensed under GPLv2. See source distribution for detailed
copyright notices.

Usage: busybox [function [arguments]...]
   or: busybox --list[-full]
   or: busybox --install [-s] [DIR]
   or: function [arguments]...

[...] 

HWINE:/data/local/tmp $
```

