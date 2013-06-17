---
layout: post
title: "GCC, Binutils Cheatsheet"
category: unix
tags: [unix, c]
---
{% include JB/setup %}

The instructions below work for both the gcc and the clang compiler driver
systems.

### Run C source code through preprocessor

    cpp main.c main.i
    clang -E main.c > main.i

### Preprocessed C source code -> Assembly

    gcc -S main.i
    clang -S main.i

### Assembly -> Relocatable object file

    gcc -c main.s
    clang -c main.s

### Link object files and produce executable object file

    gcc -o main main.o header.o
    clang main.o header.o

### Output the symbol table of an object file

    readelf --symbols main.o

### Output the ELF headers

    readelf -h main.o

### If entry point address is 0x0, object file has no 'main'

    readelf -h main.o | grep "Entry point address"