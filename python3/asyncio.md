### Python黑魔法 --- 异步IO（ asyncio） 协程
http://www.jianshu.com/p/b5e347b3a17c
https://hackernoon.com/@tryexceptpass
https://gist.github.com/rsj217/f6b9ddadebfa4a19c14c6d32b1e961f1#file-asyncio-concurency-py

from yield to await——Python协程演进过程（二）
https://zhuanlan.zhihu.com/p/27747738
#### 生成器
https://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do

https://github.com/michaelliao/learn-python3/blob/master/samples/async/async_wget2.py

https://medium.freecodecamp.org/a-guide-to-asynchronous-programming-in-python-with-asyncio-232e2afa44f6

https://medium.com/python-pandemonium/asyncio-coroutine-patterns-beyond-await-a6121486656f

https://medium.com/@yeraydiazdiaz/asyncio-coroutine-patterns-errors-and-cancellation-3bb422e961ff

https://hackernoon.com/asyncio-for-the-working-python-developer-5c468e6e2e8e

https://github.com/HackerNews/API

http://sdiehl.github.io/gevent-tutorial/

https://github.com/yeraydiazdiaz/asyncio-ftwpd

#### 理解 Python asyncio
http://lotabout.me/2017/understand-python-asyncio/

https://www.slideshare.net/saghul/asyncio-internals
#### Python 的异步 IO：Asyncio 简介（一）
https://segmentfault.com/a/1190000008814676
#### 浅析 Python 的类、继承和多态
https://segmentfault.com/a/1190000010046025
#### Python并发编程之协程/异步IO 实战 （有参考）
https://segmentfault.com/a/1190000007851357
#### [Python]在一段Python程序中使用多次事件循环
https://segmentfault.com/a/1190000011013476

http://asyncio.readthedocs.io/en/latest/producer_consumer.html
#### 使用 Python 进行并发编程 - asyncio 篇 (一、二)
https://juejin.im/post/5857b8a98e450a006cb060fc
https://juejin.im/post/585a59e4ac502e0067116b44

https://pymotw.com/3/asyncio/index.html

http://pyvideo.org/pycon-us-2017/asyncawait-and-asyncio-in-python-36-and-beyond.html

#### Python asyncio cheatsheet
https://www.pythonsheets.com/notes/python-asyncio.html

https://vorpus.org/blog/some-thoughts-on-asynchronous-api-design-in-a-post-asyncawait-world/
#### executing Tasks Concurrently
https://pymotw.com/3/asyncio/tasks.html

http://igordavydenko.com/talks/se-2016/#slide-1
http://igordavydenko.com/talks/se-2016/#slide-32

https://mdk.fr/blog/python-coroutines-with-async-and-await.html
#### 1000多页的书
https://books.google.com.hk/books?id=VW3WDQAAQBAJ&pg=PA1004&lpg=PA1004&dq=python+asyncio+await&source=bl&ots=qOEcSPybX0&sig=3b0aggCQzm8z56Sc89wOntP28Rk&hl=zh-CN&sa=X&ved=0ahUKEwiw9tXzsvDXAhUJNJQKHfwrACs4HhDoAQhZMAY#v=onepage&q=python%20asyncio%20await&f=false
#### 基于Python 3.5异步的Actor模型
http://www.malike.net.cn/blog/2016/07/03/python-3-dot-5-async-actor/
#### Python 协程从零开始到放弃
https://lightless.me/archives/python-coroutine-from-start-to-boom.html



### 控制Python异步蠕变 Controlling Python Async Creep
Python刚刚在基础语言中增加了正式的异步性。玩asyncio任务和协同程序是很有趣的，基本结构几乎是并行执行的。但是，当你开始把更多的东西集成到一个普通的代码库时，你会发现事情会变得棘手。特别是如果你被迫与同步代码交互。
调用等待函数时会出现复杂情况。这样做需要一个异步定义的代码块或协程。一个非问题，除非你的调用者必须是异步的，否则你不能调用它，除非调用者是异步的。然后强制其调用者进入异步块，等等。这是“异步蠕变”。
它可以快速升级，找到你的代码的各个角落。如果代码库是异步的，这不是什么大问题，但是在混合时会阻碍开发。
以下是我用来解决这个问题的两个主要机制。都假设我们正在构建一个从异步执行中受益的应用程序。
#### 等待异步代码块
无论是构建异步应用程序还是增强线性应用程序，确定从异步执行中获得最多的部分都是非常重要的。这通常不难回答，但没有人能为你做。一般的准则是从等待I / O的东西开始，比如文件或套接字访问，HTTP请求等。
一旦你知道哪些部分要优化，开始识别可以在彼此之上运行的部分。你越能团结在一起，越好。一个很好的例子是需要来自多个不依赖于彼此的REST API的信息的代码。您可以使用aiohttp并行进行所有呼叫，而不是等待每个人完成下一个之前。
现在将这些代码块加载到主事件循环中。有几种方法，我喜欢把它们放到异步函数中，并使用asyncio.ensure_future（）把它们放到循环中，然后使用loop.run_until_complete（）等待完成：
```
import asyncio
import aiohttp
async def fetch(url):
    response = await aiohttp.request('GET', url)
    return await response.text()
loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.gather(
    asyncio.ensure_future(fetch("http://www.google.com")),
    asyncio.ensure_future(fetch("http://www.github.com")),
    asyncio.ensure_future(fetch("http://www.reddit.com"))
))
```
这是一个类似于我以前的文章中使用的例子：螺纹异步魔术和如何使用它。
asyncio.ensure_future（）将函数变成协程，asyncio.gather（）将它们分组在一起，而loop.run_until_complete（）阻塞执行，直到所有调用完成。 这是一个包含每个调用结果的列表。
遵循以上讨论的要点将产生运行同步块的代码。 但是其中的一些块会一起执行多个异步功能。
#### 使用线程
在我之前的文章中也讨论过，创建一个独立的作为工作者的线程并不难。 它运行自己的事件循环，并使用线程安全的asyncio方法来使其工作。 好的部分是你可以用call_soon（）或者run_coroutine_threadsafe（）来同步工作。
```
from threading import Thread
...
def start_background_loop(loop):
    asyncio.set_event_loop(loop)
    loop.run_forever()
# Create a new loop
new_loop = asyncio.new_event_loop()
# Assign the loop to another thread
t = Thread(target=start_background_loop, args=(new_loop,))
t.start()
# Give it some async work
future = asyncio.run_coroutine_threadsafe(
    fetch("http://www.google.com"),
    new_loop
)
# Wait for the result
print(future.result())
# Do it again but with a callback
asyncio.run_coroutine_threadsafe(
    fetch("http://www.github.com"),
    new_loop
).add_done_callback(lambda future: print(future.result()))
```
我们从run_coroutine_threadsafe得到一个Future，我们可以使用result（timeout）方法等待，或者使用add_done_callback（function）添加一个回调函数。回调函数将接收未来作为参数。
#### 在相同的API方法中支持异步和同步调用
让我们看看更复杂的东西。如果你有一个函数库可以并行运行的库或者模块，但是如果调用者是异步的，你只想这么做。
我们可以在这里利用线程模型，因为调度器方法是同步的。这意味着你的用户不需要声明自己是异步的，并且必须处理代码中的异步蠕变。异步模块保留在模块中。
它也允许可以选择是否使用asyncio的api接口。事实上，我们甚至可以更进一步，自动检测什么时候使用一些检测魔法来进行异步。
没有线程，你无法控制你的事件循环。用户可以做自己的异步摆弄，干扰你的方法如何执行。线程至少可以保证在你操作的事件循环中执行异步执行。一个，你需要时开始和停止。这导致更可预测，可重复的结果。
我们来看一个以前面的例子。这里我们做一个包装方法，根据调用者调用相应的同步或异步函数。
```
import inspect
import requests
...
def is_async_caller():
    """Figure out who's calling."""
    # Get the calling frame
    caller = inspect.currentframe().f_back.f_back
    # Pull the function name from FrameInfo
    func_name = inspect.getframeinfo(caller)[2]
    # Get the function object
    f = caller.f_locals.get(
        func_name,
        caller.f_globals.get(func_name)
    )
    # If there's any indication that the function object is a
    # coroutine, return True. inspect.iscoroutinefunction() should
    # be all we need, the rest are here to illustrate.
    if any([inspect.iscoroutinefunction(f),
            inspect.isgeneratorfunction(f),
            inspect.iscoroutine(f), inspect.isawaitable(f),
            inspect.isasyncgenfunction(f) , inspect.isasyncgen(f)]):
        return True
    else:
        return False
def fetch(url):
    """GET the URL, do it asynchronously if the caller is async"""

    # Figure out which function is calling us
    if is_async_caller():
        print("Calling ASYNC method")
        # Run the async version of this method and
        # print the result with a callback
        asyncio.run_coroutine_threadsafe(
            _async_fetch(url),
            new_loop
        ).add_done_callback(lambda f: print(f.result()))
    else:
        print("Calling BLOCKING method")
        # Run the synchronous version and print the result
        print(_sync_fetch(url))

def _sync_fetch(url):
    """Blocking GET"""
    return requests.get(url).content

async def _async_fetch(url):
    """Async GET"""
    resp = await aiohttp.request('GET', url)
    return await resp.text()

def call_sync_fetch():
    """Blocking fetch call"""
    fetch("http://www.github.com")

async def call_async_fetch():
    """Asynchronous fetch call (no different from sync call
       except this function is defined async)"""
    fetch("http://www.github.com")

# Perform a blocking GET
call_sync_fetch()
# Perform an async GET
loop = asyncio.get_event_loop()
loop.run_until_complete(call_async_fetch())
```

我们在is_async_caller（）中使用inspect来获取调用我们的函数对象，并确定它是否是一个协程。 虽然这是幻想和说明的可能性，它可能不是很高性能。 我们可以很容易地在提取包装器中用一个async_execute参数替换机制，并让用户决定。
call_sync_fetch和call_async_fetch函数可以显示用户如何调用我们的包装器。 正如你所看到的，没有必要等待fetch调用，因为它是通过在单独的线程中运行来自动完成的。
这对任何希望增加对异步执行的支持的python包来说都是有用的，同时还支持遗留代码。 我确信有正面和反面的意见，请随时在下面的评论中开始讨论。

11111111111111111111111111111111111111111
