##PostgreSQL的libpq连接池
https://habrahabr.ru/post/322966/

为了在C ++中使用PostgreSQL，有一个很棒的libpq库。该图书馆是有据可查的，甚至从PostgresPRO完全翻译成俄文。

当编写一个服务器后端，面对这样的事实，在这个库中不存在的连接池，并与数据库工作，承担了相当密集模式和连接的一个显然是不够的。每次建立一个连接来发送接收到的数据，这将是疯狂的，因为 连接是最长的操作，因此决定编写自己的连接池。

这个想法是，当我们启动程序时，我们创建了几个连接并将它们存储在队列中。

当数据到来时，我们就从队列中获得自由soedineie，如果没有自由的连接-等待的时候会有，用它来插入数据，然后把连接返回。这个想法很简单，很快实施，最重要的是工作速度非常快。

在PostgreSQL中创建一个名为demo的演示数据库
```c
-- Table: public.demo

-- DROP TABLE public.demo;

CREATE TABLE public.demo
(
  id integer NOT NULL DEFAULT nextval('demo_id_seq'::regclass),
  name character varying(256),
  CONSTRAINT demo_pk PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.demo
  OWNER TO postgres;
```
写一个类将是将代码简化它直接登记到数据库连接设置的连接，在现实中，他们当然应该被存放在一个配置文件，并从那里开始阅读改变parmetrov服务器不必重新编译程序。 


pgconnection.h
pgconnection.cpp
为了防止可能的资源泄漏，我们将把连接存储在一个智能指针中。

在构造函数中，我们调用PQsetdbLogin函数，该函数建立与数据库的连接，将指针返回到PGconn *连接并将连接转换为异步操作模式。

完成后，必须通过PQfinish函数删除连接，PQsetdbLogin函数返回的指针将被传递给该函数。因此，调用m_connection.reset（）中的最后一个参数是＆PQfinish函数的地址。当智能指针退出示波器并重置参考计数时，它将调用该函数，从而正确终止连接。

现在我们需要一个将创建，存储和管理连接池的工作的类。

pgbackend.h
pgbackend.cpp
在createPool函数中，我们创建了一个连接池，我建立了10个连接。接下来，创建一个PGBackend类，并通过连接函数（它返回一个到数据库的空闲连接）和freeConnection（它将连接放回队列）来处理它。

所有这些都是在条件变量的基础上工作的，如果队列是空的，那么就没有空闲的连接，并且线程一直睡着，直到它通过条件变量被唤醒。

在main.cpp文件中给出了最简单的例子，在这个例子中，我们使用了带有连接池的后端。在“战斗条件”下，你当然会有一些循环的事件，在进行数据库工作的同时，在我这个boost :: asio异步工作，并接受来自网络的事件，所有写入数据库。把它带到这里是不必要的，以免把这个想法与一个连接池复杂化。在这里，我们简单地创建50个线程，通过PGBackend的一个实例与服务器一起工作。

main.cpp中
这是通过命令编译的：

g++ main.cpp pgbackend.cpp pgconnection.cpp -o pool -std=c++14 -I/usr/include/postgresql/ -lpq -lpthread

请注意数据库连接的数量 - 此参数由参数max_connections（integer）指定。

源代码https://github.com/borisovs/pool
