[Back](./Chapter-0.md)

# Necessary Packages

We will use our alpine container to compile the kernel and other necessary pieces of software. Later on, we also borrow some dynamic libraries from it. Therefore not all the installed packages are needed for building the kernel.



## [bash](https://pkgs.alpinelinux.org/package/v3.16/main/x86/bash)

[Bash](https://www.gnu.org/software/bash/bash.html) is a sh-compatible shell that we rely on both for building our operating system and for its feature development.

## [bc](https://pkgs.alpinelinux.org/package/v3.16/main/x86/bc)

[bc](https://www.gnu.org/software/bc/bc.html) is an arbitrary precision numeric processing language and [its needed to build the kernel](https://unix.stackexchange.com/questions/439482/why-is-bc-required-to-build-the-linux-kernel).

## [bison](https://pkgs.alpinelinux.org/package/v3.16/main/x86/bison)

[Bison](https://www.gnu.org/software/bison/bison.html) is a general-purpose parser generator. It is typically used along with [flex](#flex) for creating programs in charge of lexical analysis, which is one of the steps when compiling high-level languages

## [build-base](https://pkgs.alpinelinux.org/package/v3.16/main/x86/build-base)

It is meta package for intalling a collection of packages needed for software development in C/C++ including:
* [binutils](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/binutils) 
* [file](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/file)
* [gcc](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/gcc) 
* [g++](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/g++) 
* [make](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/make) 
* [libc-dev](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/libc-dev) 
* [fortify-headers](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/fortify-headers)
* [patch](https://pkgs.alpinelinux.org/package/v3.16/main/x86_64/patch)

## [cpio](https://pkgs.alpinelinux.org/package/v3.16/community/x86/cpio)

It is used to copies files into/out of [cpio](https://www.gnu.org/software/cpio/) or tar archives.

## [flex](https://pkgs.alpinelinux.org/package/v3.16/main/x86/flex)

[flex](https://github.com/westes/flex) is a lexical analyzer generator. It is typically used along with [Bison](#bison) for creating programs in charge of lexical analysis, which is one of the steps when compiling high-level languages

## [libelf](https://pkgs.alpinelinux.org/package/v3.16/main/x86/libelf)

libelf contains the runtime libraries of [elfutils](https://sourceware.org/elfutils/) that is a collection of utilities and libraries to read, create and modify ELF binary files, find and handle DWARF debug data, symbols, thread state and stacktraces for processes and core files on GNU/Linux.

## [libelf-static](https://pkgs.alpinelinux.org/package/v3.16/main/x86/libelf-static)

The statically compiled version of libelf. It contains the runtime in thr format of ".a" instead of ".so" files.

## [elfutils-dev](https://pkgs.alpinelinux.org/package/v3.16/main/x86/elfutils-dev)

Contains necessary files (including headers) for using elfutils runtime in development.

## [ncurses-dev](https://pkgs.alpinelinux.org/package/v3.16/main/x86/ncurses-dev)

[ncurses](https://invisible-island.net/ncurses/) is a library to provide programmers with an API for terminal-independent TUI (text-based user interface) development. ncurses-dev contains ".so" and header files to use the library in development. It is needed to configure the Linux kernel using the "menuconfig" option.

## [ncurses-static](https://pkgs.alpinelinux.org/package/v3.16/main/x86/ncurses-static)

Contains the statically linked files (".a" files) of ncurses.

## [openssl-dev](https://pkgs.alpinelinux.org/package/v3.16/main/x86/openssl-dev)

The [OpenSSL](https://www.openssl.org/) software is full-featured toolkit for general-purpose cryptography and secure communication and openssl-dev package contains teh header files for using it in development.

## [openssl-libs-static](https://pkgs.alpinelinux.org/package/v3.16/main/x86/openssl-libs-static)

Contains the statically linked files (".a" files) of [OpenSSL](https://www.openssl.org/).

## [linux-headers](https://pkgs.alpinelinux.org/package/v3.16/main/x86/linux-headers)

This package contains the Linux kernel header files, which are needed for building the "menuconfig" option while configuring the kernel. The ncurses-based configuration of kernel needs them and not compiling the kernel itself.

## [xz](https://pkgs.alpinelinux.org/package/v3.16/main/x86/xz)

The algorithm/application used for the kernel image's compression depends on the chosen configuration. We will use the LZMA algorithm, and consequently, the kernel's build system will use the [xz](https://tukaani.org/xz/) application.

## [vim](https://pkgs.alpinelinux.org/package/v3.16/main/x86/vim)

It shouldn't need any explanation.