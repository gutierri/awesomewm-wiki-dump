---
title: Awesome-3-RHEL5
permalink: /Awesome-3-RHEL5/
---

Building Awesome 3 on Red Hat Enterprise Linux 5 is a lot of work: almost the entire X client library has to be rebuilt to get the new and shiny features that Awesome wants. This script does the job (for the specific versions of each component listed), and builds everything in a local prefix, assuming that you don't have root access.

` #!/bin/bash`
` `
` # Building awesome.`
` #`
` # References:`
` #   `[`http://awesome.naquadah.org/wiki/Awesome-3-RHEL6`](http://awesome.naquadah.org/wiki/Awesome-3-RHEL6)
` #       Build instructions for RHEL6; not quite complete for RHEL5!`
` #`
` #   `[`http://www.freedesktop.org/wiki/Software`](http://www.freedesktop.org/wiki/Software)
` `
` PREFIX="$(cd "$(dirname "$0")"; pwd)"`
` `
` extract_cd()`
` {`
`     cd "$PREFIX/src"  &&`
`     case "$1" in`
`     *.tar.gz)   app_name="${1%.tar.gz}";    x=x ;;`
`     *.tar.bz2)  app_name="${1%.tar.bz2}";   x=j ;;`
`     $)          echo >&2 Whoops ;;`
`     esac  &&`
`     tar x${x}f "$PREFIX/tars/$1"  &&`
`     cd "$app_name"`
` }`
` `
` install_pc()`
` {`
`     sed "/^prefix[ \t]*=/s:=.*$:=$PREFIX:" \`
`         <"$1" >"$PKG_CONFIG_PATH/$2"`
` }`
` `
` configure_build()`
` {`
`     ./configure --prefix="$PREFIX" $1  &&`
`     make  &&`
`     make install`
` }`
` `
` build()`
` {`
`     extract_cd "$1"  &&`
`     configure_build $2`
` }`
` `
` build_lua()`
` {`
`     extract_cd "$1"  &&`
`     patch -p1 <"$PREFIX/tars/lua.patch"  &&`
`     make linux  &&`
`     make install INSTALL_TOP="$PREFIX"  &&`
`     install_pc etc/lua.pc lua5.1.pc`
` }`
` `
` build_awesome()`
` {`
`     extract_cd "$1"  &&`
`     mkdir .build  &&`
`     cd .build  &&`
`     cmake -DCMAKE_INSTALL_PREFIX="$PREFIX" . ..  &&`
`     make  &&`
`     make install`
` }`
` `
` `
` export PKG_CONFIG_PATH="$PREFIX/lib/pkgconfig"  &&`
` mkdir -p "$PKG_CONFIG_PATH" "$PREFIX/src"  &&`
` PATH="$PREFIX/bin:$PATH"  &&`
` `
` build gperf-3.0.4.tar.gz  &&`
` build imlib2-1.4.4.tar.gz  &&`
` build libev-4.01.tar.gz  &&`
` install_pc "$PREFIX/tars/ev.pc" ev.pc  &&`
` build libpthread-stubs-0.3.tar.gz  &&`
` build xproto-7.0.20.tar.gz  &&`
` build xcb-proto-1.6.tar.gz  &&`
` build libxcb-1.7.tar.gz  &&`
` build xcb-util-0.3.6.tar.gz  &&`
` build libxdg-basedir-1.1.1.tar.gz  &&`
` build pixman-0.21.2.tar.gz  &&`
` build startup-notification-0.10.tar.gz  &&`
` `
` build freetype-2.4.4.tar.gz  &&`
` build fontconfig-2.8.0.tar.gz  &&`
` build gettext-0.18.1.1.tar.gz  &&`
` build glib-2.26.1.tar.gz  &&`
` build cairo-1.8.10.tar.gz --enable-xcb  &&`
` build pango-1.28.3.tar.gz  &&`
` `
` build_lua lua-5.1.4.tar.gz  &&`
` `
` build_awesome awesome-3.4.8.tar.bz2`

This script should be placed in the prefix directory, and all the source tar files placed in a tars subdirectory. Running this script requires CMake 2.8.x (the default on RHEL5 is 2.6.x) to compile awesome-3.8.4, without which it will fail with the following message:

` CMake Error at CMakeLists.txt:284 (file):`
`   file does not recognize sub-command COPY`

You can obtain a CMake RPM for your RHEL from here: [CMake on RepoForge](http://pkgs.repoforge.org/cmake/)

In this same directory place the following two patch files:

tars/ev.pc:

` prefix=/scratch/awesome`
` exec_prefix=${prefix}`
` libdir=${exec_prefix}/lib`
` includedir=${prefix}/include`
` `
` Name: ve`
` Description: Event loop library`
` Version: 4.01`
` Libs: -L${libdir} -lev`
` Cflags: -I${includedir}`
` `

tars/lua.patch:

` diff -ur lua-5.1.4/Makefile lua-5.1.4-patched/Makefile`
` --- lua-5.1.4/Makefile    2008-08-12 01:40:48.000000000 +0100`
` +++ lua-5.1.4-patched/Makefile    2010-12-10 17:23:46.000000000 +0000`
` @@ -43,7 +43,7 @@`
`  # What to install.`
`  TO_BIN= lua luac`
`  TO_INC= lua.h luaconf.h lualib.h lauxlib.h ../etc/lua.hpp`
` -TO_LIB= liblua.a`
` +TO_LIB= liblua.a liblua.so`
`  TO_MAN= lua.1 luac.1`
`  `
`  # Lua version and release.`
` diff -ur lua-5.1.4/src/Makefile lua-5.1.4-patched/src/Makefile`
` --- lua-5.1.4/src/Makefile    2008-01-19 19:37:58.000000000 +0000`
` +++ lua-5.1.4-patched/src/Makefile    2010-12-10 17:32:04.000000000 +0000`
` @@ -8,7 +8,7 @@`
`  PLAT= none`
`  `
`  CC= gcc`
` -CFLAGS= -O2 -Wall $(MYCFLAGS)`
` +CFLAGS= -O2 -Wall $(MYCFLAGS) -fPIC`
`  AR= ar rcu`
`  RANLIB= ranlib`
`  RM= rm -f`
` @@ -28,6 +28,7 @@`
`   lundump.o lvm.o lzio.o`
`  LIB_O=   lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o \`
`   lstrlib.o loadlib.o linit.o`
` +LUA_SO = liblua.so`
`  `
`  LUA_T=   lua`
`  LUA_O=   lua.o`
` @@ -36,7 +37,7 @@`
`  LUAC_O=  luac.o print.o`
`  `
`  ALL_O= $(CORE_O) $(LIB_O) $(LUA_O) $(LUAC_O)`
` -ALL_T= $(LUA_A) $(LUA_T) $(LUAC_T)`
` +ALL_T= $(LUA_A) $(LUA_T) $(LUAC_T) $(LUA_SO)`
`  ALL_A= $(LUA_A)`
`  `
`  default: $(PLAT)`
` @@ -57,6 +58,9 @@`
`  $(LUAC_T): $(LUAC_O) $(LUA_A)`
`   $(CC) -o $@ $(MYLDFLAGS) $(LUAC_O) $(LUA_A) $(LIBS)`
`  `
` +$(LUA_SO): $(CORE_O) $(LIB_O)`
` + $(CC) -o $@ -shared $?`
` +`
`  clean:`
`   $(RM) $(ALL_T) $(ALL_O)`