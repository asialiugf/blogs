### qt 静态编译



```c
set Path=C:\Qt\Qt5.9.3\Tools\mingw530_32\opt\bin;C:\Qt\Qt5.9.3\Tools\mingw530_32\bin
configure.bat -static -debug-and-release -platform win32-g++ -prefix C:\Qt\Qt5.9.3\5.9.3Static  -qt-zlib -qt-pcre -qt-libpng -qt-libjpeg -qt-freetype -opengl desktop -no-openssl  -opensource -confirm-license  -make libs -nomake tools -nomake examples -nomake tests
mingw32-make
mingw32-make install
```
