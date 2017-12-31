### g++_pkg-config了解.md

http://blog.csdn.net/luotuo44/article/details/24836901

```c
riddle@d:/usr/local/lib/pkgconfig$ 
riddle@d:/usr/local/lib/pkgconfig$ vi libmongocxx.pc
# Copyright 2016 MongoDB Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

prefix=/usr/local
includedir=${prefix}/include
libdir=${prefix}/lib

Name: libmongocxx
Description: The MongoDB C++11 Driver Library
URL: http://github.com/mongodb/mongo-cxx-driver
Version: 3.2.0-rc1
Requires: libbsoncxx >= 3.2.0-rc1
Cflags: -I${includedir}/mongocxx/v_noabi
Libs: -L${libdir} -lmongocxx
~                                                                                                                     
~                                                                                                                     
~                                                                                                                     
~                                                                                                                     
riddle@d:/usr/local/lib/pkgconfig$ ll
total 100
-rw-r--r-- 1 root root 338 Dec 10 11:59 gflags.pc
-rw-r--r-- 1 root root 269 Dec 10 11:51 gmock_main.pc
-rw-r--r-- 1 root root 262 Dec 10 11:51 gmock.pc
-rw-r--r-- 1 root root 285 Dec 10 11:51 gtest_main.pc
-rw-r--r-- 1 root root 262 Dec 10 11:51 gtest.pc
-rw-r--r-- 1 root root 248 Dec 31 14:37 libbson-1.0.pc
-rw-r--r-- 1 root root 838 Dec 31 15:02 libbsoncxx.pc
-rw-r--r-- 1 root root 917 Dec 25 13:09 libczmq.pc
-rw-r--r-- 1 root root 282 Dec 10 11:42 libevent_core.pc
-rw-r--r-- 1 root root 285 Dec 10 11:42 libevent_extra.pc
-rw-r--r-- 1 root root 412 Dec 10 11:42 libevent_openssl.pc
-rw-r--r-- 1 root root 318 Dec 10 11:42 libevent.pc
-rw-r--r-- 1 root root 355 Dec 10 11:42 libevent_pthreads.pc
-rw-r--r-- 1 root root 227 Dec 10 11:31 libglog.pc
-rw-r--r-- 1 root root 315 Dec 31 14:37 libmongoc-1.0.pc
-rw-r--r-- 1 root root 230 Dec 31 14:37 libmongoc-ssl-1.0.pc
-rw-r--r-- 1 root root 877 Dec 31 15:02 libmongocxx.pc
-rw-r--r-- 1 root root 269 Dec 29 13:05 libpcrecpp.pc
-rw-r--r-- 1 root root 278 Dec 29 13:05 libpcre.pc
-rw-r--r-- 1 root root 311 Dec 29 13:05 libpcreposix.pc
-rw-r--r-- 1 root root 493 Dec 11 16:05 libssh2.pc
-rw-r--r-- 1 root root 238 Dec 12 18:34 libwebsockets.pc
-rw-r--r-- 1 root root 273 Dec 12 18:34 libwebsockets_static.pc
-rw-r--r-- 1 root root 244 Dec 10 20:16 libzmq.pc
-rw-r--r-- 1 root root 311 Dec 24 21:28 nanomsg.pc
riddle@d:/usr/local/lib/pkgconfig$ 
```
