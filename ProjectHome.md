### What's the disadvantages of the offical MinGW 4.4.0 ###
#### 1. gdb can't reach the breakpoints in the header files when debugging under CB due to a bug in GCC. ####
#### 2. debugging cross models like if you want to debug DLLs, you need to "disable" the "catch C++ exception options" in CB, otherwise your gdb debugger will exit abnormally. ####
### What's the advantages of the offical MinGW 4.4.0 ###
#### 1. A lot of fixes in STL libraries. ####
#### 2. Better handling of dw-2 exceptions. ####
#### 3. Better handling of Windows SEH exceptions. ####

### What's the advantage of TDM GCC 4.4.1 ###
#### 1. it fixes the bug when breakpoint can't be reached in header files. ####
#### 2. catch exceptions functionality can be debugged safely cross models, and you don't need to Disable "catch c++ exception" in CB. ####
#### 3. support large file handing. ####
#### 4. added feature:-std=c++0x option, also, fix build error when macro STRICT\_ANSI defined. ####
#### 5. added feature: stack is extended to 32MB ####
#### 6. all other fixes... ####

### What's the disadvantages of MinGW TDM 4.4.1 ###
#### 1. It doesn't have the official MinGW patches included. ####
#### 2. the official GCC is now 4.4.4, and TDM MinGW GCC is still 4.4.1. ####

###### Also: both the TDM MinGW GCC and the official MinGW is build under cygwin or native Windows. ######
###### MinGW QP GCC&G++ release was corss-built from Linux. ######

### What's the advantages of QP's MinGW GCC 4.4.5 ###
#### It integrates all the patches from offcial mingw and TDM GCC 4.4.1. So, we have both the advantages from these previous MinGW releases, and the disadvantages of the QP's GCC release were minimized. ####

### What's the advantages of QP's MinGW GCC 4.4.5 ###
#### We have do a lot of testing before our release, these testing include building and debugging wxWidgets2.8.10, Qt4.6.2, Code::Blocks. But we don't guarantee this release is "bug free", so, we welcome the user to report any bugs. ####

### Download packages ###
You can either download a "full MinGW packages" or only the "GCC&G++ package".
A full version
which includes:
  * QP's GCC&G++ 4.4.3 release
  * binutils-2.20.1-2
  * gdb-7.0.50
  * make-3.81
  * mingwrt-3.18,
  * w32api- 3.14,
  * pthreads2.8,
  * iconv\_1.13.1
can be downloaded from mingw32-4.4.5-all.7z
### Manual installation ###
If you would like to manually install, you can only download the QP's GCC&G++ packages here:
gcc\_g++-4.4.3.7z
Then, you need to download other packages from the MinGW offcial site, these include:
  * binutils
  * mingw-runtime (devand dll)
  * w32api
  * Required runtime libraries (pthreads)
  * mingw-gdb for debugger
  * mingw32-make for make
Just extract these package to the same folder.

Translation by Asmwarrior

# Additional infomation #
Project member Leo (a14331990) also maintains another MinGW GCC project,
http://code.google.com/p/a1mingwgcc/,
focused on cut-edge MinGW GCC versions.
Please help testing them.