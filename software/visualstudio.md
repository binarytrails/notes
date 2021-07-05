# visualstudio

## c++

1. Add libarary headers (.h,.hpp) into your lib folder
    
    C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC\XX.YY.ZZZZZ\include\mylib

2. Add compiled library files into your project

    Right click on project → Properties → VS → Properties → Linker → General → Additional library directory → Add
    "C:\lib\mylib\lib\Debug" and "C:\lib\mylib\Release"

3. Add compiled library to linker path

    Linker → Additional Dependencies → mylib.lib
