* https://blog.csdn.net/cmbug/article/details/49339341
```
经过上面这一长篇大论，是时候该总结一下成果了：


多个源文件可同属于一个包，只要声明时package指定的包名一样；
一个包对应生成一个*.a文件，生成的文件名并不是包名+.a组成，应该是目录名+.a组成
go install ××× 这里对应的并不是包名，而是路径名！！
import ××× 这里使用的也不是包名，也是路径名
×××××.SayHello() 这里使用的才是包名！
指定×××路径名就代表了此目录下唯一的包，编译器连接器默认就会去生成或者使用它，而不需要我们手动指明！
一个目录下就只能有一个包存在
对于调用有源码的第三方包，连接器在连接时，其实使用的并不是我们工作目录下的.a文件，而是以该最新源码编译出的临时文件夹中的.a文件
对于调用没有源码的第三方包，上面的临时编译不可能成功，那么临时目录下就不可能有.a文件，所以最后链接时就只能链接到工作目录下的.a文件
对于标准库，即便是修改了源代码，只要不重新编译Go源码，那么链接时使用的就还是已经编译好的*.a文件
包导入有三种模式：正常模式、别名模式、简便模式

```

### 静态库
* http://reborncodinglife.com/2018/04/27/how-to-create-static-lib-in-golang/
* https://golangtc.com/t/5ae145684ce40d091ecf04bf
```
.a文件可以调用，已经解决。假设 hello.a 在当前文件夹，其他程序叫 main.go

go tool compile -I . main.go
go tool link -o main -L . main.o

具体可以参考：http://reborncodinglife.com/2018/04/27/how-to-create-static-lib-in-golang/
```
