
## 9.类
类提供了将数据和功能捆绑在一起的方法。创建一个新类创建一个新类型的对象，允许创建该类型的新实例。每个类实例都可以附加属性以保持其状态。类实例也可以有方法（由它的类定义）来修改它的状态。

与其他编程语言相比，Python的类机制以最少的新语法和语义添加类。它是C ++和Modula-3中的类机制的混合体。 Python类提供了面向对象编程的所有标准功能：类继承机制允许多个基类，派生类可以重写其基类或类的任何方法，而方法可以调用具有相同名称的基类的方法。对象可以包含任意数量和种类的数据。和模块一样，类也是Python的动态特性的一部分，它们是在运行时创建的，可以在创建后进一步修改。

在C ++术语中，通常类成员（包括数据成员）是公共的（除了见下面的私有变量），所有成员函数都是虚拟的。和Modula-3一样，从它的方法中引用对象的成员没有简短的方法：方法函数是用明确的第一个参数来声明的，这个第一个参数代表对象，这个对象由调用隐含地提供。就像在Smalltalk中一样，类本身就是对象。这为导入和重命名提供了语义。与C ++和Modula-3不同的是，内置类型可以被用作用户扩展的基类。而且，就像在C ++中一样，大多数具有特殊语法（算术运算符，下标等）的内置运算符都可以为类实例重新定义。

（缺乏普遍接受的术语来谈论类，我偶尔会使用Smalltalk和C ++术语，我会用Modula-3术语，因为它的面向对象的语义比Python更接近于Python的术语，但我希望很少有读者听说过。）

### 9.1。 关于名称和对象的一个词
对象具有个性，并且可以将多个名称（在多个作用域中）绑定到同一个对象。 这被称为其他语言中的别名。 这通常不会被人们理解，在处理不可变的基本类型（数字，字符串，元组）时可以安全地忽略它。 但是，别名对涉及可变对象（如列表，字典和大多数其他类型）的Python代码的语义可能会有惊人的影响。 这通常用于程序的好处，因为在某些方面别名的行为类似于指针。 例如，传递一个对象是便宜的，因为只有一个指针被实现传递; 如果一个函数修改了一个作为参数传递的对象，调用者将会看到这个改变 - 这就消除了在Pascal中需要两个不同的参数传递机制。

### 9.2。 Python作用域和命名空间
在介绍类之前，我首先要告诉你一些关于Python的作用域规则。类定义对命名空间起着一些巧妙的把戏，你需要知道范围和命名空间是如何工作的，以便充分理解正在发生的事情。顺便提一下，关于这个主题的知识对于任何高级Python程序员都是有用的。

我们从一些定义开始。

名称空间是从名称到对象的映射。大多数命名空间目前都是以Python字典的形式实现的，但通常不会以任何方式引起注意（性能除外），并且将来可能会发生变化。命名空间的例子是：一组内置的名字（包含像abs（）这样的函数和内置的异常名字）;模块中的全局名称;和函数调用中的本地名称。在某种意义上，对象的属性集合也构成一个名称空间。了解名称空间的重要之处在于，不同名称空间中的名称之间绝对没有关系。例如，两个不同的模块都可以定义最大化的功能而不会混淆 - 模块的用户必须以模块名称作为前缀。

顺便说一下，我使用单词属性来表示任意一个点的名称 - 例如，在表达式z.real中，real是对象z的一个属性。严格地说，对模块中名称的引用是属性引用：在表达式modname.funcname中，modname是模块对象，funcname是它的一个属性。在这种情况下，模块的属性和模块中定义的全局名称之间会有直接的映射关系：它们共享相同的名称空间！ [1]

属性可以是只读的或可写的。在后一种情况下，可以分配属性。模块属性是可写的：你可以写modname.the_answer = 42。可写属性也可以用del语句删除。例如，del modname.the_answer会从modname命名的对象中删除属性the_answer。

命名空间是在不同的时刻创建的，具有不同的生命周期。包含内置名称的命名空间是在Python解释器启动时创建的，永远不会被删除。读取模块定义时创建模块的全局名称空间;通常，模块名称空间也会持续到解释器退出。由解释器的顶层调用执行的语句，无论是从脚本文件读取还是交互式读取，都被视为__main__模块的一部分，因此它们拥有自己的全局名称空间。 （内置的名字实际上也存在于一个模块中，这被称为builtins。）

函数的本地名称空间在调用函数时创建，并在函数返回时删除，或引发在函数中未处理的异常。 （实际上，遗忘是描述实际情况的更好方法。）当然，递归调用每个都有自己的本地名称空间。

范围是Python程序的文本区域，可以直接访问命名空间。这里的“直接访问”意味着对名称的非限定引用会尝试在名称空间中查找名称。

虽然范围是静态确定的，但它们是动态使用的。在执行期间的任何时候，至少有三个嵌套的作用域可直接访问：

- 最先搜索的最里面的范围包含本地名称
- 从最近的封闭范围开始搜索的任何封闭函数的范围包含非本地名称，也包含非全局名称
- 倒数第二个作用域包含当前模块的全局名称
- 最后一个范围（最后搜索）是包含内置名称的名称空间

如果一个名字被声明为全局的，那么所有的引用和赋值直接进入包含模块全局名字的中间作用域。要重新绑定在最内层范围之外发现的变量，可以使用非本地语句;如果没有声明为nonlocal，那么这些变量是只读的（试图写入这样的变量将只是在最内层的范围内创建一个新的局部变量，而保持相同的外层变量不变）。

通常，本地作用域引用（文本）当前函数的本地名称。在外部函数中，本地作用域引用与全局作用域相同的名称空间：模块的名称空间。类定义在本地作用域中放置另一个名称空间。

意识到范围是由文本决定的：在模块中定义的函数的全局范围是该模块的名称空间，而不管从哪里或通过别名调用函数。另一方面，名字的实际搜索是在运行时动态完成的 - 然而，语言定义正在向“编译”时间的静态名称解析发展，所以不要依赖动态名称解析！ （事实上​​，局部变量已经静态确定了。）

Python的一个特别的问题是，如果没有全局语句生效，对名称的分配总是进入最内层的范围。 分配不会复制数据 - 它们只是将名称绑定到对象。 删除也是如此：语句del x从本地作用域引用的名称空间中删除了x的绑定。 实际上，所有引入新名称的操作都使用本地作用域：特别是import语句和函数定义将模块或函数名称绑定在本地作用域中。

全球声明可以用来表示特定的变量生活在全球范围内，应该在那里反弹; 非本地声明表明特定的变量生活在一个封闭的范围内，应该在那里反弹。

#### 9.2.1。 范围和命名空间示例
这是一个演示如何引用不同范围和名称空间的示例，以及全局和非本地如何影响变量绑定：
```
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```
示例代码的输出是：
```
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```
请注意，本地分配（默认情况下）不会更改scope_test对垃圾邮件的绑定。 非本地分配改变了scope_test对垃圾邮件的绑定，全局分配改变了模块级绑定。

您还可以看到在全局分配之前没有以前的垃圾邮件绑定。

### 9.3。 首先看类
类引入了一些新的语法，三种新的对象类型和一些新的语义。

#### 9.3.1。 类定义语法
类定义最简单的形式如下所示：
```
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```
类定义，比如函数定义（def语句）必须在它们有效之前被执行。 （你可以想象将一个类定义放在一个if语句的分支中，或者在一个函数中。

实际上，类定义中的语句通常是函数定义，但是其他语句是允许的，有时也是有用的 - 我们稍后再回来。一个类中的函数定义通常有一个特殊的参数列表形式，由方法的调用约定来决定 - 这也在后面解释。

当输入一个类定义时，会创建一个新的名称空间，并将其用作本地作用域 - 因此，所有对局部变量的赋值都将进入这个新的名称空间。特别是，函数定义在这里绑定新函数的名字。

当类定义保持正常（通过结束）时，创建一个类对象。这基本上是由类定义创建的命名空间内容的一个包装;我们将在下一节中学习更多有关类对象的知识。原来的本地作用域（在输入类定义之前生效的那个作用域）被恢复，类对象在这里被绑定到类定义头（在这个例子中是ClassName）给出的类名。

#### 9.3.2。类对象
Class对象支持两种操作：属性引用和实例化。

属性引用使用Python中用于所有属性引用的标准语法：obj.name。有效的属性名称是创建类对象时在类名称空间中的所有名称。所以，如果类定义如下所示：
```
class MyClass:
    """A simple example class"""
    i = 12345

    def f(self):
        return 'hello world'
```
那么MyClass.i和MyClass.f是有效的属性引用，分别返回一个整数和一个函数对象。 类属性也可以分配给，所以你可以通过赋值来改变MyClass.i的值。 __doc__也是一个有效的属性，返回属于该类的文档字符串：“一个简单的示例类”。

类实例使用函数表示法。 假设类对象是一个返回类的新实例的无参数函数。 例如（假设上面的类）：
```
x = MyClass()
```
创建该类的新实例并将该对象分配给局部变量x。

实例化操作（“调用”一个类对象）创建一个空对象。 许多类喜欢创建具有自定义特定初始状态的对象的对象。 因此，一个类可能会定义一个名为__init __（）的特殊方法，如下所示：
```
def __init__(self):
    self.data = []
```
当一个类定义了一个__init __（）方法时，类实例自动为新创建的类实例调用__init __（）。 所以在这个例子中，一个新的，初始化的实例可以通过以下方式获得：
```
x = MyClass()
```
当然，__init __（）方法可能有更多的灵活性参数。 在这种情况下，赋给类实例化操作符的参数被传递给__init __（）。 例如，
```
>>> class Complex:
...     def __init__(self, realpart, imagpart):
...         self.r = realpart
...         self.i = imagpart
...
>>> x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```
#### 9.3.3。 实例对象
现在我们可以用实例对象做什么？ 实例对象理解的唯一操作是属性引用。 有两种有效的属性名称，数据属性和方法。

数据属性对应于Smalltalk中的“实例变量”，以及C ++中的“数据成员”。 数据属性不需要声明; 像局部变量一样，它们在第一次分配时就会弹出。 例如，如果x是上面创建的MyClass的实例，则下面的一段代码将打印值16，而不留下任何痕迹：
```
x.counter = 1
while x.counter < 10:
    x.counter = x.counter * 2
print(x.counter)
del x.counter
```
另一种实例属性引用是一种方法。 一个方法是一个“属于”一个对象的功能。 （在Python中，术语方法并不是唯一的类实例：其他对象类型也可以有方法，例如，列表对象有append，insert，remove，sort等方法，但是在下面的讨论中， 除非另外明确说明，否则我们将使用术语“方法”专门指类实例对象的方法。

实例对象的有效方法名取决于它的类。 根据定义，作为函数对象的类的所有属性定义其实例的对应方法。 所以在我们的例子中，x.f是一个有效的方法引用，因为MyClass.f是一个函数，但是x.i不是，因为MyClass.i不是。 但是x.f与MyClass.f不是一回事 - 它是一个方法对象，而不是函数对象。

#### 9.3.4。 方法对象
通常，一个方法在被绑定之后被调用：
```
x.f()
```
在MyClass示例中，这将返回字符串“hello world”。 但是，不需要立即调用一个方法：x.f是一个方法对象，可以在稍后存储和调用。 例如：
```
xf = x.f
while True:
    print(xf())
```
将继续打印你好世界，直到时间的尽头。

当一个方法被调用时究竟发生了什么？您可能已经注意到x.f（）在上面没有参数的情况下被调用，即使f（）的函数定义指定了一个参数。发生了什么事？当一个需要参数的函数没有被调用时，Python会引发一个异常，即使这个参数实际上没有被使用。

其实，你可能已经猜到了答案：关于方法的特殊之处在于实例对象作为函数的第一个参数传递。在我们的例子中，调用x.f（）和MyClass.f（x）完全等价。一般情况下，调用带有n个参数列表的方法相当于用通过在第一个参数之前插入方法实例对象创建的参数列表来调用相应的函数。

如果你仍然不明白方法是如何工作的，那么查看实现可能会澄清一些事情。当引用不是数据属性的实例属性时，将搜索其类。如果名称表示一个有效的类属性是一个函数对象，则方法对象是通过打包实例对象和在一个抽象对象中一起找到的函数对象（指针）来创建的：这是方法对象。当使用参数列表调用方法对象时，会从实例对象和参数列表构造一个新的参数列表，并使用此新参数列表调用函数对象。

#### 9.3.5。类和实例变量
一般来说，实例变量是针对每个实例唯一的数据，而类变量则是针对类的所有实例共享的属性和方法：
```
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
>>> d.name                  # unique to d
'Fido'
>>> e.name                  # unique to e
'Buddy'
```
例如，以下代码中的技巧列表不应该被用作类变量，因为只有一个列表 将由所有Dog实例共享：
```
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```
类的正确设计应该使用一个实例变量：
```
class Dog:

    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks
['roll over']
>>> e.tricks
['play dead']
```
### 9.4。随机备注
为了避免意外的名称冲突，在大型程序中可能导致难以发现的错误，使用某种约定来减少冲突的可能性是明智的。可能的约定包括大写方法名称，用一个小的唯一字符串（可能只是一个下划线）为数据属性名称加前缀，或者使用动词来表示数据属性的方法和名词。

事实上，Python中没有任何东西可以强制执行数据隐藏。事实上，没有任何东西被方法引用，也没有被方法引用，也没有引用对象的普通用户（“客户端”）。 - 这一切都是基于约定的（这是用C语言编写的Python的扩展）

请注意，客户端可能会添加数据属性，请注意，客户端可能会将自己的数据属性添加到实例对象，而不会影响方法的有效性，只要名称冲突得以避免 - 再一次，命名规则可以在这里节省很多麻烦。

没有引用数据属性（或其他方法！）的简写方法。在通过方法浏览时，不会混淆局部变量和实例变量。

这只不过是一个约定：名称self对于Python来说绝对没有特别的意义，但是请注意，如果不遵循约定，那么对其他Python程序员，并且也可以想象，可以编写依赖于这种约定的类浏览器程序程序。

。任何功能对象，它是一类属性定义该类这是没有必要的是，函数定义文本地封闭在类定义:.分配一个功能对象到在类的局部变量的实例的方法也行。例如：
```
# Function defined outside the class
def f1(self, x, y):
    return min(x, x+y)

class C:
    f = f1

    def g(self):
        return 'hello world'

    h = g
```
注意这个练习只是一个给一个程序的读者的表白。

方法可以使用自变量的方法属性来调用其他方法：
```
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)
```
（一个类永远不会被用作全局范围。）虽然很少遇到使用全局数据的好理由 在一个方法中，很多情况下，全局范围中包含的函数可以被方法使用，也可以被用作在其中定义的类。 它是在这个全球范围内定义的，在下一节中，我们找到了一些很好的理由，为什么一个方法想引用自己的类。

它存储为对象.__ class__。

### 9.5。继承
当然，在不支持继承的情况下，语言特性将不值得使用“class”这个名称。派生类定义的语法如下所示：
```
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```
名称Base className必须在包含派生类定义的作用域中定义，而不是基类名称，也允许使用其他任意表达式。例如，当在另一个模块中定义基类时，这是很有用的：
```
class DerivedClassName(modname.BaseClassName):
```
这用于解析属性引用：如果在类中找不到请求的属性，则搜索进度如果基类本身是从其他类派生的，则递归地应用此规则。

有没有什么特别的派生类的实例：DerivedClassName（）创建一个类的方法引用的新实例如下：对应的类属性进行搜索，沿基类，如果必要的链条都解决了，方法引用是有效的如果这产生一个函数对象。

派生类可以覆盖基类的方法。因为方法调用同一对象的其它方法时没有特殊的特权，调用在相同的基类中定义的另一方法的基类的方法可最终调用的派生的方法类覆盖它（对于C ++程序员：Python中的所有方法都是有效的）。

有一个简单的方法直接调用基类方法：只需调用BaseClassName.methodname（self，arguments）。这对于客户端来说也是有用的（注意，这只有在基类可以在全局范围内作为BaseClassName访问时才有效）。

Python有两个与继承一起工作的内置函数：

使用isinstance（）来检查实例的类型：isinstance（OBJ，INT）将是真实的，只有当obj .__ class__为int或者从int派生类的一些。
，因为float不是int的子类，所以Issubclass（float，int）是False。
#### 9.5.1。多重继承
具有多个基类的类定义如下所示：
```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```
因此，它在层级中是重叠的，因此在层级中是重叠的。 ，如果属性不DerivedClassName找到，它被搜索于Base1，然后（递归地）在Base1的基类，并且如果未发现有它，将其搜索在和Base2，依此类推。

事实上，这是稍微复杂一些比;.方法解析顺序是动态变化的，以支持超（）这个方法是在其他一些多继承的语言，例如呼叫下一个方法称为合作调用和更强大的比超调用在单继承语言中找到。

例如，所有的类都从对象继承，所以任何情况下都是多重继承提供一个以上的路径到达目标。要被访问一次以上保持基类，动态算法线性化的方式搜索顺序，保留在每类中指定的左到右的顺序，调用各家长只有一次，那就是单调的（意思是一类可以在不影响其父母的优先顺序子类）。总之，这些特性使其能够与多重继承设计可靠和可扩展的类。欲了解更多详情，请参阅HTTPS ：//www.python.org/download/releases/2.3/mro/。

### 9.6。私有变量
。不能只是从对象中访问Python中不存在但是“私有”实例变量，有后跟大多数Python代码约定：以下划线前缀（如_spam）的名称应被视为一个的API（无论它是一个函数，方法或数据成员）的非公开的一部分。应当考虑的实现细节并随时更改而不通知。

由于存在一个有效的用例为类私有成员（即，以避免名称的名称冲突与由子类定义名称），存在对这样的机制支持有限，称为名称重整。形式__spam的任何标识符（至少只要它发生，这个前导的修改就完全不考虑标识符的语法位置在一个类的定义之内。

名称修改有助于让子类重写方法而不会破坏类内方法调用，例如：
```
class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # private copy of original update() method

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # provides new signature for update()
        # but does not break __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```
请注意，mangling规则大多是为了避免事故而设计的，例如甚至是在调试器中的情况。

请注意，传递给exec（）或eval（）的代码不会将调用类的类名称视为当前类;其作用同样局限于字节 一起编译，同样的限制适用于getattr（），setattr（）和delattr（），以及直接引用__dict__。

### 9.7。赔率和结束
一个空的类定义可以很好地完成：有时候，有一个类似于Pascal“记录”或C“结构”的数据类型是非常有用的，它将几个命名的数据项绑定在一起。一个空的类定义将很好地做到：
```
class Employee:
    pass

john = Employee()  # Create an empty employee record

# Fill the fields of the record
john.name = 'John Doe'
john.dept = 'computer lab'
john.salary = 1000
```
一段需要特定抽象数据类型的Python代码可以被传递一个模拟该类型类型的方法的类。 使用方法read（）和readline（）来代替字符串缓冲区中的数据，并将其作为参数传递。

实例方法对象也有属性：m .__ self__是m（）方法的实例对象，m .__ func__是方法对应的函数对象。

### 9.8。迭代器
到目前为止，您可能已经注意到大多数容器对象可以使用for语句循环：
```
for element in [1, 2, 3]:
    print(element)
for element in (1, 2, 3):
    print(element)
for key in {'one':1, 'two':2}:
    print(key)
for char in "123":
    print(char)
for line in open("myfile.txt"):
    print(line, end='')
```
这种访问方式清晰，简洁，方便。迭代器的使用遍历和统一Python。在幕后，for语句在容器对象上调用iter（）。  该函数返回一个迭代器对象，该对象定义一次访问容器中元素的方法__next __（）。当没有更多的元素时，__next __（）引发一个StopIteration异常，它告诉for循环终止。您可以使用next（）内置函数调用__next __（）方法;  这个例子展示了它是如何工作的：
```
>>> s = 'abc'
>>> it = iter(s)
>>> it
<iterator object at 0x00A1DB50>
>>> next(it)
'a'
>>> next(it)
'b'
>>> next(it)
'c'
>>> next(it)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    next(it)
StopIteration
```
看到了迭代器协议后面的机制，很容易将迭代器行为添加到类中。如果类定义了__next __（），那么__iter __（）可以返回self：
```
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]
```
```
>>> rev = Reverse('spam')
>>> iter(rev)
<__main__.Reverse object at 0x00A1DB50>
>>> for char in rev:
...     print(char)
...
m
a
p
s
```
### 9.9。生成器
生成器是创建迭代器的简单而强大的工具。 它们像常规函数一样编写，但是只要他们想返回数据就使用yield语句。 每当next（）被调用时，生成器会从停止的地方恢复（它会记住所有的数据值以及上次执行的语句）。 一个例子表明，生成器可以很容易地创建：
```
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
>>>
```
```
>>> for char in reverse('golf'):
...     print(char)
...
f
l
o
g
```
任何可以用发生器完成的事情也可以使用前面部分描述的基于类的迭代器来完成。什么使得生成器如此紧凑，是自动创建__iter __（）和__next __（）方法。

另一个关键特性是在调用之间自动保存局部变量和执行状态。这使得该函数更容易编写，比使用self.index和self.data等实例变量的方法更清晰。

除了自动方法创建和保存程序状态之外，当发生器终止时，它们会自动提高StopIteration。结合起来，这些功能可以让您轻松创建迭代器，而不需要编写常规函数。

9.10。生成器表达式
一些简单的生成器可以使用类似于列表解析的语法简洁地编码为表达式，但是括号而不是括号。这些表达式适用于通过封闭函数立即使用生成器的情况。生成器表达式比完整的生成器定义更紧凑但功能更少，往往比等效的列表解析更具有内存友好性。

例子：
```
>>> sum(i*i for i in range(10))                 # sum of squares
285

>>> xvec = [10, 20, 30]
>>> yvec = [7, 5, 3]
>>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
260

>>> from math import pi, sin
>>> sine_table = {x: sin(x*pi/180) for x in range(0, 91)}

>>> unique_words = set(word  for line in page  for word in line.split())

>>> valedictorian = max((student.gpa, student.name) for student in graduates)

>>> data = 'golf'
>>> list(data[i] for i in range(len(data)-1, -1, -1))
['f', 'l', 'o', 'g']
```
#### 脚注

[1]除了一件事。 模块对象有一个叫做__dict__的秘密只读属性，它返回用于实现模块名称空间的字典; 名称__dict__是一个属性，但不是全局名称。 很明显，使用这样做违反了命名空间实现的抽象，应该被限制在事后调试器之类的东西。
