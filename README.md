opam-android-repository
=======================

This fork of vouillon's repository was developed to support [Mirage](http://www.openmirage.org/) on Android.

This OPAM repository contains an OCaml cross-compiler for Google
Android, as well as a few OCaml libraries and programs compiled for
Android.

It also includes android toolchain versions of various mirage dependencies.

Installation notes
------------------

Use the following command to add this repository to the list of
repositories used by OPAM:
```
opam repo add android https://github.com/cgreenhalgh/opam-android-repository.git
```

The following command will list the available packages:
```
opam list | grep android
```

Building some modules requires a patched ocamlbuild (to use ocamlfind for ocamlmklib):
```
ocaml switch 4.00.1+mirage-android
```

Some notes on building Mirage for Android are in the
[open sharing toolkit](https://github.com/cgreenhalgh/opensharingtoolkit/blob/master/docs/mirageonandroid.md)
project documentation.

The Android NDK (Native Development Kit) is automatically downloaded.

On a 64bit Debian or Ubuntu installation, you need to install
package `gcc-multilib`, as we have to build 32 bit OCaml binaries
when targeting 32 bit architectures.

Directory structure
-------------------

Cross-compilation tools use the `arm-linux-androideabi-` prefix (for
instance, `arm-linux-androideabi-ocamlc`) to differentiate them from
standard OCaml tools. For convenience, unprefixed binaries are also
available in directory `%{bin}%/arm-linux-androideabi/`, where
`%{bin}%` is the directory where OPAM normally puts binaries
(typically, `~/.opam/4.00.1/bin/` for OCaml 4.00.1).

Ocamlfind can be invoked as either:
- `ocamlfind -toolchain android`,
- `arm-linux-androideabi-ocamlfind`,
- `%{bin}%/arm-linux-androideabi/ocamlfind`.

The Android NDK is in `%{lib}%/android-ndk` where `%{prefix}%` is the
directory under which OPAM normally puts libraries (typically,
`~/.opam/4.00.1/lib/` for OCaml 4.00.1).  The C compiler is called
with the right options when invoked through the OCaml compilers, so
this location only matters if you need to invoke the compiler
directly. See the build script for package `android-c-openssh` for an
example of how to do it.

Android libraries and binaries are placed in directory
`%{prefix}%/arm-linux-androideabi`, where `%{prefix}%` is the
directory under which OPAM normally puts libraries and libraries
(typically, `~/.opam/4.00.1/` for OCaml 4.00.1). For instance,
the path of the unison binary is:
```
%{prefix}/arm-linux-androideabi/bin/unison
```

Bytecode programs
-----------------

There are a few pitfalls regarding bytecode programs.  First, if you
link them without the `-custom` directive, you will need to use
`ocamlrun` explicitly to run them. Second, the `ocamlmklib` command
produces shared libraries `dll*.so` which are not usable. Thus, you
need to use the `-custom` directive to successfully link bytecode
programs that uses libraries with mixed C / OCaml code. Shared
libraries should eventually be disabled, but at the moment, the
`ocamlbuild` plugin of `oasis` requires them to be created.

Acknowledgements
----------------

Many thanks to Keigo Imai for his OCaml 3.12 cross-compiler patches.
