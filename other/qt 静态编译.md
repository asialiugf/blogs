### qt 静态编译

#### 下载qt，并安装在 C:\Qt\Qt5.9.3 目录下
http://download.qt.io/

http://download.qt.io/official_releases/qt/5.9/5.9.3/
在这里下载 
qt-opensource-windows-x86-5.9.3.exe

并安装在 C:\Qt\Qt5.9.3 这个目录下

在C:\Qt\Qt5.9.3目录下，建一个文件夹 5.9.3Static,再在这个目录下建一个src目录

下载http://download.qt.io/official_releases/qt/5.9/5.9.3/single/qt-everywhere-opensource-src-5.9.3.zip
放在新建的src目录下，并解压缩





#### 静态链接参考
https://wiki.qt.io/Building_a_static_Qt_for_Windows_using_MinGW



```c
set Path=C:\Qt\Qt5.9.3\Tools\mingw530_32\opt\bin;C:\Qt\Qt5.9.3\Tools\mingw530_32\bin
configure.bat -static -debug-and-release -platform win32-g++ -prefix C:\Qt\Qt5.9.3\5.9.3Static  -qt-zlib -qt-pcre -qt-libpng -qt-libjpeg -qt-freetype -opengl desktop -no-openssl  -opensource -confirm-license  -make libs -nomake tools -nomake examples -nomake tests
mingw32-make
mingw32-make install
```
