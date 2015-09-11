This wiki page tells you the steps to build MinGW GCC 4.4.3 compiler.
# build gmp #
```
DIR=/home/loaden/AUR/mingw32
ABI=32 ../../src/gmp-5.0.1/configure \
    --host=mingw32 \
    --target=mingw32 \
    --build=i686-pc-linux-gnu \
    --enable-cxx \
    --disable-static \
    --enable-shared
make && make install prefix=$DIR/gmp && mingw32-strip $DIR/gmp/bin/*
```

# build mpfr #
You need to use the option: --with-gmp, otherwise, you can't generate DLL files.

```
DIR=/home/loaden/AUR/mingw32
../../src/mpfr-2.4.2/configure \
    --host=mingw32 \
    --target=mingw32 \
    --build=i686-pc-linux-gnu \
    --with-gmp=$DIR/gmp \
    --disable-static \
    --enable-shared
make && make install prefix=$DIR/mpfr && mingw32-strip $DIR/mpfr/bin/*
```

# build gcc #
```
#apply patches to the GCC source, run the command under GCC source root.
for files in ../patches/gcc*; do patch -p0 < $files; done

# in/mingw folder, create link.(run the command under target folder; or you can copy these command, and run them simultaneously)

sudo mkdir /mingw
sudo ln -s /usr/mingw32/include/ /mingw/include
sudo ln -s /usr/mingw32/lib/ /mingw/lib

#define variables
DIR=/home/loaden/AUR/mingw32
GCC_VER=4.4.3

#configure the libgomp, pthread path
mkdir -p ./mingw32/libgomp
echo \
CFLAGS=\"-I$DIR/pthreads/include -O2 -mthreads -D__USE_MINGW_ACCESS\"$'\n'\
LDFLAGS=\"-L$DIR/pthreads/lib\"$'\n'\
> ./mingw32/libgomp/config.cache

# export the environment variables
export C_INCLUDE_PATH="$DIR/pthreads/include"
export CPLUS_INCLUDE_PATH="$DIR/pthreads/include"
export LIBRARY_PATH="$DIR/pthreads/lib"
export LD_LIBRARY_PATH="$DIR/pthreads/bin"
export CFLAGS="-I$DIR/pthreads/include -O2 -D__USE_MINGW_ACCESS"
export BOOT_CFLAGS="-O2 -D__USE_MINGW_ACCESS"
export CFLAGS_FOR_TARGET="-O2 -D__USE_MINGW_ACCESS"
export CXXFLAGS="-I$DIR/pthreads/include -mthreads -O2 -D__USE_MINGW_ACCESS"
export BOOT_CXXFLAGS="-mthreads -O2 -D__USE_MINGW_ACCESS"
export CXXFLAGS_FOR_TARGET="-mthreads -O2 -D__USE_MINGW_ACCESS"
export LDFLAGS="-s" BOOT_LDFLAGS="-s"
export LDFLAGS_FOR_TARGET="-L$DIR/pthreads/lib -s"

# configure
../../src/gcc-$GCC_VER/configure \
    --prefix=/mingw \
    --host=mingw32 \
    --target=mingw32 \
    --build=i686-pc-linux-gnu \
    --enable-languages=c,c++ \
    --enable-cxx-flags='-fno-function-sections -fno-data-sections' \
    --enable-fully-dynamic-string \
    --enable-version-specific-runtime-libs \
    --enable-threads=win32 \
    --enable-libgomp \
    --disable-nls \
    --disable-libstdcxx-pch \
    --disable-win32-registry \
    --disable-sjlj-exceptions \
    --with-dwarf2 \
    --with-gmp=$DIR/gmp \
    --with-mpfr=$DIR/mpfr \
    --with-pkgversion='QP MinGW32' \
    --with-bugurl=http://www.qpsoft.com/blog/guestbook.php
make

# -Wl,...: these options tell gcc that the following options will be passed to linker.
# add opiton: --stack=0x2000000 , so that the stack value was extended to 32MB to avoid the stack overflow.

rm -f gcc/cc1.exe gcc/cc1plus.exe
make install DESTDIR=$DIR LDFLAGS="-s -Wl,--stack=0x2000000"
```


# build gcc fix libgomp (updated 2010 03 15) #
```
# configure
../../src/gcc-4.4.3/configure \
    --prefix=/mingw \
    --host=mingw32 \
    --target=mingw32 \
    --build=i686-pc-linux-gnu \
    --enable-languages=c,c++ \
    --enable-shared \
    --enable-cxx-flags='-fno-function-sections -fno-data-sections' \
    --enable-fully-dynamic-string \
    --enable-version-specific-runtime-libs \
    --enable-threads=win32 \
    --enable-libgomp \
    --disable-nls \
    --disable-libstdcxx-pch \
    --disable-win32-registry \
    --disable-sjlj-exceptions \
    --with-dwarf2 \
    --with-gmp=$DIR/gmp \
    --with-mpfr=$DIR/mpfr \
    --with-pkgversion='QP MinGW32' \
    --with-bugurl=http://www.qpsoft.com/blog/guestbook.php
make CFLAGS="-O2 -D__USE_MINGW_ACCESS" \
    BOOT_CFLAGS="-O2 -D__USE_MINGW_ACCESS" \
    CFLAGS_FOR_TARGET="-I$DIR/pthreads/include -O2 -D__USE_MINGW_ACCESS" \
    CXXFLAGS="-mthreads -O2 -D__USE_MINGW_ACCESS" \
    BOOT_CXXFLAGS="-mthreads -O2 -D__USE_MINGW_ACCESS" \
    CXXFLAGS_FOR_TARGET="-mthreads -O2 -D__USE_MINGW_ACCESS" \
    LDFLAGS="-s" BOOT_LDFLAGS="-s" \
    LDFLAGS_FOR_TARGET="-L$DIR/pthreads/lib -s"
```

```
# libgomp error handling: You need the change mingw32/libgomp/libtool script, rename the ".lib" to ".dll.a", there are two places in the script:

# 1. library_names_spec="\${libname}\`echo \${release} | \$SED -e s/[.]/-/g\`\${versuffix}\${shared_ext} \$libname.lib"
# 2. reload_objs="$objs$old_deplibs "`$ECHO "X$libobjs" | $SP2NL | $Xsed -e '/\.'${libext}$'/d' -e '/\.lib$/d' -e "$lo2o" | $NL2SP`" $reload_conv_objs" ### testsuite:
# redo it again, then you can build successfully!
```