
### python3学习文档
http://docspy3zh.readthedocs.io/en/latest/index.html
https://docs.python.org/3/library/asyncio.html
http://www.pythondoc.com/pythontutorial3/
https://docs.python.org/3/download.html

#### 基于 Boost 库实现的 Python 调用 C++ 接口
https://zhuanlan.zhihu.com/thephdgrind/24059497
#### Ubuntu下编写python c++ 扩展
http://www.linuxidc.com/Linux/2009-12/23585.htm


#### 使用C语言扩展Python3
https://www.cnblogs.com/yaoyu126/p/6435912.html
```c
apt-get update
apt-get update --fix-missing
apt-get install python3-dev


g++ -o libmypyext.so -export-dynamic -m64 -shared -I/usr/include/python3.5m demo.c -fPIC

demo.c
#include <Python.h>

// c function
static PyObject *
demo_system(PyObject *self, PyObject *args) {
    const char *command;
    int sts;
    if (!PyArg_ParseTuple(args, "s", &command))
        return NULL;
    sts = system(command);
    return PyLong_FromLong(sts);
}

static PyObject *
demo_hello(PyObject *self, PyObject *args) {
    PyObject *name, *result;
    if (!PyArg_ParseTuple(args, "U:demo_hello", &name))
        return NULL;
    result = PyUnicode_FromFormat("Hello, %S!", name);
    return result;
}

static PyObject *
demo_chinese(PyObject *self, PyObject *args) {
    char *name;
    int age;
    if (!PyArg_ParseTuple(args, "si", &name, &age))
        return NULL;
    // printf("%d\n", age);
    char total[10000];
    memset(total, 0, sizeof(total));
    strcat(total, "strcat() 函数用来连接字符串：");
    strcat(total, "tset");
    PyObject *result = Py_BuildValue("s", total);
    return result;
}

// method table
static PyMethodDef DemoMethods[] = {
    {"system", // python method name
     demo_system, // matched c function name
     METH_VARARGS, /* a flag telling the interpreter the calling
                                convention to be used for the C function. */
     "I guess here is description." },

     {"hello", demo_hello,  METH_VARARGS, "I guess here is description." },
     {"chinese", demo_chinese, METH_VARARGS, NULL },
     {NULL, NULL, 0, NULL}        /* Sentinel */
};

// The method table must be referenced in the module definition structure.
static struct PyModuleDef demomodule = {
    PyModuleDef_HEAD_INIT,
    "demo",   /* name of module */
    NULL, /* module documentation, may be NULL */
    -1,       /* size of per-interpreter state of the module,
                or -1 if the module keeps state in global variables. */
    DemoMethods
};

// The initialization function must be named PyInit_name()
PyMODINIT_FUNC
PyInit_demo(void)
{
    return PyModule_Create(&demomodule);
}

g++ -o libmypyext.so -export-dynamic -m64 -shared -I/usr/include/python3.5m demo.c -fPIC
cp libmypyext.so demo.so

hello.py
import demo
print("---------------------------")
status = demo.system("ls -l")
print("---------------------------")
hi = demo.hello("Sink")
print(hi)
print("---------------------------")
hi = demo.chinese("Sink", 2014)
print(hi)
print("---------------------------")

setup.py
#setup.py
from distutils.core import setup, Extension

module1 = Extension('demo',
                    sources = ['demo.c'])

setup (name = 'Demo hello',
       version = '1.0',
       description = 'This is a demo package by Sink',
       ext_modules = [module1])
#end----------

```
##### Python协程：从yield/send到async/await
http://python.jobbole.com/86069/
##### 使用 Python 进行并发编程 - asyncio 篇 (三)
https://juejin.im/post/586729e3b123db005dddccd9
如何让 Python 像 Julia 一样快地运行
http://blog.csdn.net/u013597671/article/details/72593896


用Cython加速Python程序
https://www.cnblogs.com/yafengabc/p/6130849.html
Cython基础--C结构体，枚举，以及常量在Cython中的定义和使用
http://blog.csdn.net/i2cbus/article/details/18456129
http://blog.csdn.net/mmc2015/article/details/51914599
15.11 用Cython写高性能的数组操作
http://python3-cookbook.readthedocs.io/zh_CN/latest/c15/p11_use_cython_to_write_high_performance_array_operation.html
http://python3-cookbook.readthedocs.io/zh_CN/latest/c15/p10_wrap_existing_c_code_with_cython.html
https://pandas.pydata.org/pandas-docs/stable/enhancingperf.html
Python科学计算：Cython基础
https://wklchris.github.io/Py3-Cython-basic.html

python – C代码与cython的简单包装
https://stackoverflow.com/questions/3046305/simple-wrapping-of-c-code-with-cython
####  在python里调用C函数的三种方式
https://www.yanxurui.cc/posts/python/3-ways-of-calling-c-functions-from-python/
