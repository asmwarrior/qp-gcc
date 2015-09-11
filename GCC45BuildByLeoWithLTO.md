# Introduction #

Link Time Optimization (LTO) is a new optimization strategy in GCC 4.5, it works great. See
GCC wiki page [1](1.md) for details.

LTO on non-ELF platform is implemented/patched by Dave Korn [2](2.md), his patch was already in trunk, but not in GCC 4.5.0, he kindly provided a patch for GCC 4.5.0 [2](2.md).

[1](1.md) http://gcc.gnu.org/wiki/LinkTimeOptimization

[2](2.md) GCC Bugzilla [Bug 42776](https://code.google.com/p/qp-gcc/issues/detail?id=2776) LTO doesn't work on non-ELF platforms.

http://gcc.gnu.org/bugzilla/show_bug.cgi?id=42776


# Details #

# Build Notes #
  * Only two patches, coff-section-directive-alignment.diff (not necessary if binutils 2.20.1) and gcc\_lto-coff-respin-4.diff, are needed.
  * Apply patches as normal, but **don't forget to run autoconf 2.64 at srcdir and srcdir/gcc**, **autoconf before 2.63 is not working, for autoconf 2.65 or later, simple modifications on srcdir/configure.ac and srcdir/config/override.m4 are needed.**
  * autoconf in msys maybe broken, try to run autoconf in a Linux environment or Cygwin shell, and move the autoconf'd source code to a msys environment for further deployments.
  * replace --disable-lto with --enable-lto in gcc configure step.