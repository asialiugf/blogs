
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->
<!-- code_chunk_output -->

* [libuv github家](#libuv-github家)
* [libuv 使用中的一些个人认识](#libuv-使用中的一些个人认识)
* [libuv 使用点滴](#libuv-使用点滴)
* [libuv之线程池的坑，注意避免](#libuv之线程池的坑注意避免)
* [libuv源码分析(2)](#libuv源码分析2)

#### 一个线程 一个 event_loop，在这个loop中，回调函数不能是死循环。（2018-09-02）
#### Linux下的I/O复用与epoll详解 (mmap、红黑树、链表)
* https://www.cnblogs.com/lojunren/p/3856290.html
* http://blog.51cto.com/11418774/1836638

<!-- /code_chunk_output -->

#### libuv github家
https://github.com/libuv/libuv

#### libuv 使用中的一些个人认识
1. 如果用libuv，那就尽量都用它，循环，文件读写，线程等；

2. 只有uv_async_t是线程安全的，配套uv_async_init(loop, &async, async_cb);      uv_async_send(&async);
        4. async_cb中必须uv_close((uv_handle_t*)&async, close_cb);   //如果async没有关闭，消息队列是会阻塞的，async是监视器的一种

3. 线程ID 获取uv_thread_t id = uv_thread_self();

4. 不要在回调函数中设置死循环，否则会导致其它回调无法执行！（个人感觉，libuv的工作都是以工作项形式进行的挂靠到loop上的，一个接着一个的处理）
5. 创建子进程用spawn，不要fork（曾遇到fork的子进程中调用uv_queue_work，不执行，原因不知到，有懂的前辈请留言指教， 有个前辈给我留言说libuv不应该与fork一起用）

6. 句柄和buf部分，不要使用局部变量，会损坏栈数据，导致程序崩溃！
（亲身经历，网上有些人发布的代码，句柄使用的局部变量，而没有用static或malloc，虽然运行一下没问题，但一会就会出问题的！
    查了半天自己的代码查不出问题，去看libuv实现，uv_write和uv_run的实现，发现里面有工作队列queue，void*类型的，直接赋值和使用，没有另开辟存储，所以如果是局部变量的话，在加入工作队列时，对应内存就无效了，还去使用，会损坏栈，导致崩溃！）

#### libuv 使用点滴
http://blog.csdn.net/bywayboy/article/details/47137209
#### libuv之线程池的坑，注意避免
http://blog.csdn.net/wangcg123/article/details/52386024
#### libuv源码分析(2)
https://www.cnblogs.com/watercoldyi/p/5682344.html
