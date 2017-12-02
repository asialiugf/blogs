
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
```
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
