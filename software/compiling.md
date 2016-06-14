# Makefile

> The problem with Dennis’s Makefile is that when he added the comment line, he inadvertently inserted a space before the tab character at the beginning of line 2. The tab character is a very important part of the syntax of Makefiles. All command lines (the lines beginning with cc in our example) must start with tabs. -- The Unix-Haters Handbook

# GCC (GNU Compiler Collection)

## Using on C++

Sooner or later you will encounter weird error like:

    undefined reference to `__gxx_personality_v0'

Instead, use *g++* or append *-lstdc++* to the gcc command:

    gcc class.cpp -lstdc++

## Linking order

> GCC's linker is a one pass linker which means the order of library flags matters. You want to list the libraries in reverse dependency order. In other words, if a depends on b, then b must appear before a in the linker flags.

