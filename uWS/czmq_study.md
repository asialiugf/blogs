#### uuid-dev 安装 -luuid
```
 apt-get install  uuid-dev 
```
#### czmq 安装
https://github.com/zeromq/czmq/releases

从以上地址下载，解压然后执行
```
./configure
make
make check
make install
```
```python
Configure complete! Now proceed with:
    - 'make'               compile the project
    - 'make check'         run the project's selftest
    - 'make install'       install the project to /usr/local

Further options are:
    - 'make callcheck'     run the project's selftest with valgrind to
                           check for performance leaks
    - 'make check-verbose' run the project's selftest in verbose mode
    - 'make code'          generate code from models in src directory
                           (requires zproject and zproto)
    - 'make debug'         run the project's selftest under gdb
    - 'make memcheck'      run the project's selftest with valgrind to
                           check for memory leaks
    - 'make coverage'      generate project's selftest coverage report
                           expects --with-gcov option for configure

```
