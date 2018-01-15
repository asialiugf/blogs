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

设置window10的环境变量：
控制面板\系统和安全\系统  选择  高级系统设置  然后 修改系统设置 
```c
set Path=C:\Qt\Qt5.9.3\Tools\mingw530_32\opt\bin;C:\Qt\Qt5.9.3\Tools\mingw530_32\bin
```
然后进入到命令行 CMD
进入以下目录 C:\Qt\Qt5.9.3\5.9.3Static\src\qt-everywhere-opensource-src-5.9.3\qtbase

在这个目录下执行configure.bat 参数如下：
```c
configure.bat -static -debug-and-release -platform win32-g++ -prefix C:\Qt\Qt5.9.3\5.9.3Static  -qt-zlib -qt-pcre -qt-libpng -qt-libjpeg -qt-freetype -opengl desktop -no-openssl  -opensource -confirm-license  -make libs -nomake tools -nomake examples -nomake tests
```
然后执行
```c
mingw32-make
mingw32-make install
```
在编译时，会报错，有些.h文件找不到，src下仔细找到后，修改相应的 include路径，再编译。

#### 静态链接参考
https://wiki.qt.io/Building_a_static_Qt_for_Windows_using_MinGW
https://github.com/aalex420/QtStatic


```c
set Path=C:\Qt\Qt5.9.3\Tools\mingw530_32\opt\bin;C:\Qt\Qt5.9.3\Tools\mingw530_32\bin
configure.bat -static -debug-and-release -platform win32-g++ -prefix C:\Qt\Qt5.9.3\5.9.3Static  -qt-zlib -qt-pcre -qt-libpng -qt-libjpeg -qt-freetype -opengl desktop -no-openssl  -opensource -confirm-license  -make libs -nomake tools -nomake examples -nomake tests
mingw32-make
mingw32-make install
```

###　Qt creator 静态编译设置

打开Qt creator ， 工具 选项  Qt version  , 新建

选择 C:\Qt\Qt5.9.3\5.9.3Static\bin\qmake.exe

然后再到 工具 选项 构建套件KIT 中，新建 一个， 取名 MyStatic，并选择Qt的版本为，刚才新建立的这个qmake.exe版本。


