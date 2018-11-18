## Problem
``apt update`` consistently failed with ``Bus error (core dumped)``
## Cause
Corrupted cache file - ``/var/cache/apt/srcpkgcache.bin``
## What I did
- Some google searches suggested to remove all cached files under ``/var/cache/apt/``
- Tried to figure out what exactly goes wrong using ``strace`` - no luck
- Set ``ulimit -c unlimited`` so that core file could be generated, and reran the command above 
- Tried to analyze coredump with ``gdb`` - no debug symbols, no luck
- Ran strings on core dump, and greped for cache
```
$ strings core | grep cache
/var/cache/apt/srcpkgcache.bin
/usr/lib/i386-linux-gnu/gconv/gconv-modules.cache
var/cache/apt/
srcpkgcache.bin
srcpkgcache
pkgcache.bin
pkgcache
/var/cache/apt/srcpkgcache.bin
/var/cache/apt/pkgcache.bin
Hiba a(z) /var/cache/apt/srcpkgcache.bin f
gcache.bin
gcache.bin
gcache.bin
/var/cache/apt/pkgcache.bin
/var/cache/apt/srcpkgcache.bin
Hiba a(z) /var/cache/apt/pkgcache.bin f
/var/cache/apt/
/var/cache/apt/
/var/cache/apt/
kgcache.[
```
(Hiba means Error)
- Deleted ``/var/cache/apt/srcpkgcache.bin``
