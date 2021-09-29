# visualstudio

## c++

### add library

1. Add libarary headers (.h,.hpp) into your lib folder
    
    C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC\XX.YY.ZZZZZ\include\mylib

2. Add compiled library files into your project

    Right click on project → Properties → VS → Properties → Linker → General → Additional library directory → Add
    "C:\lib\mylib\lib\Debug" and "C:\lib\mylib\Release"

3. Add compiled library to linker path

    Linker → Input → Additional Dependencies → mylib.lib

### include everything

In order to include the linked libraries, we need to make it statically compiled.

For this we need to:

1. Solution Properties > C/C++ > Code Generation > Runtime Library

2. Set it to /MT (use the multithread, static version of the run-time library)

https://docs.microsoft.com/en-us/cpp/build/reference/md-mt-ld-use-run-time-library?view=msvc-160

### disable treat warnings as errors

    Project properties -> configurations properties -> C/C++ -> treats warning as error -> No (/WX-)

### remove secure warnings (_CRT_SECURE_NO_WARNINGS)

    #pragma warning(disable:4996)
