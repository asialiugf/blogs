
# Python教程

## 9.类
类提供了将数据和功能捆绑在一起的方法。创建一个新类创建一个新类型的对象，允许创建新类型的实例。每个类实例都可以附加属性以保持其状态。类实例也可以有方法（由它的类定义）来修改它的状态。

与其他编程语言相比，Python的类机制以最少的新语法和语义添加类。它是C ++和Modula-3中的类机制的混合体。 Python类提供了面向对象编程的所有标准功能：类继承机制允许多个基类，派生类可以重写其基类或类的任何方法，而方法可以调用具有相同名称的基类的方法。对象可以包含任意数量和种类的数据。和模块一样，类也是Python的动态特性的一部分，它们是在运行时创建的，可以在创建后进一步修改。

在C ++术语中，通常类成员（包括数据成员）是公共的（除了见下面的私有变量），所有的成员函数都是虚拟的。 和Modula-3一样，从它的方法中引用对象的成员没有简短的方法：方法函数是用明确的第一个参数来声明的，这个第一个参数代表对象，这个对象由调用隐含地提供。 就像在Smalltalk中一样，类本身就是对象。 这为导入和重命名提供了语义。 与C ++和Modula-3不同的是，内置类型可以被用作用户扩展的基类。 而且，就像在C ++中一样，大多数具有特殊语法（算术运算符，下标等）的内置运算符都可以为类实例重新定义。

（缺乏普遍接受的术语来谈论类，我偶尔会使用Smalltalk和C ++术语，我会用Modula-3术语，因为它的面向对象的语义比Python更接近于Python的术语，但我希望很少有读者 听说过。）

### 9.1 关于名称和对象的一个词

对象具有个性，并且可以将多个名称（在多个作用域中）绑定到同一个对象。这被称为其他语言中的别名。这通常不会被人们理解，在处理不可变的基本类型（数字，字符串，元组）时可以安全地忽略它。但是，别名对涉及可变对象（如列表，字典和大多数其他类型）的Python代码的语义可能会有惊人的影响。这通常用于程序的好处，因为在某些方面别名的行为类似于指针。例如，传递一个对象是便宜的，因为只有一个指针被实现传递;如果一个函数修改了一个作为参数传递的对象，调用者将看到这个变化 - 这就消除了在Pascal中需要两个不同的参数传递机制。

### 9.2 Python作用域和命名空间

在介绍类之前，我首先要告诉你一些关于Python的作用域规则。类定义对命名空间起着一些巧妙的把戏，你需要知道范围和命名空间是如何工作的，以便充分理解正在发生的事情。顺便提一下，关于这个主题的知识对于任何高级Python程序员都是有用的。

我们从一些定义开始。

名称空间是从名称到对象的映射。大多数命名空间目前都是作为Python字典来实现的，但是通常不会以任何方式引起注意（性能除外），并且将来可能会发生变化。命名空间的例子是：一组内置的名字（包含像abs（）这样的函数和内置的异常名字）;模块中的全局名称;和函数调用中的本地名称。在某种意义上，对象的属性集合也构成一个名称空间。了解名称空间的重要之处在于，不同名称空间中的名称之间绝对没有关系。例如，两个不同的模块都可以定义最大化的功能而不会混淆 - 模块的用户必须以模块名称作为前缀。

顺便说一句，我使用单词属性作为任何名称后面的点 - 例如，在表达式z.real中，real是对象z的属性。严格地说，对模块中名称的引用是属性引用：在表达式modname.funcname中，modname是模块对象，funcname是它的一个属性。在这种情况下，模块的属性和模块中定义的全局名称之间会有直接的映射关系：它们共享相同的名称空间！ [1]

属性可以是只读的或可写的。在后一种情况下，可以分配属性。模块属性是可写的：你可以写modname.the_answer = 42。可写属性也可以用del语句删除。例如，del modname.the_answer会从modname命名的对象中删除属性the_answer。

命名空间是在不同的时刻创建的，具有不同的生命周期。包含内置名称的命名空间是在Python解释器启动时创建的，永远不会被删除。读取模块定义时创建模块的全局名称空间;通常，模块名称空间也会持续到解释器退出。由解释器的顶层调用执行的语句，无论是从脚本文件读取还是交互式读取，都被视为__main__模块的一部分，因此它们具有自己的全局名称空间。 （内置的名字实际上也存在于一个模块中，这被称为内建的。）

函数的本地名称空间在调用函数时创建，并在函数返回时删除，或引发在函数中未处理的异常。 （实际上，遗忘是描述实际情况的更好方法。）当然，递归调用每个都有自己的本地名称空间。

范围是Python程序的文本区域，可以直接访问命名空间。这里的“直接访问”意味着对名称的非限定引用会尝试在名称空间中查找名称。

虽然范围是静态确定的，但它们是动态使用的。在执行期间的任何时候，至少有三个嵌套的作用域可直接访问：

- 最先搜索的最里面的范围包含本地名称
- 从最近的封闭范围开始搜索的任何封闭函数的范围包含非本地名称，也包含非全局名称
- 倒数第二个作用域包含当前模块的全局名称
- 最后一个范围（最后搜索）是包含内置名称的名称空间

如果一个名称被声明为全局的，那么所有的引用和赋值直接进入包含该模块全局名称的中间作用域。要重新绑定在最内层范围之外发现的变量，可以使用非本地语句;如果没有声明为nonlocal，那么这些变量是只读的（试图写入这样一个变量将只是在最内层的范围内创建一个新的局部变量，保持相同的名称外层变量不变）。

通常，本地作用域引用（文本）当前函数的本地名称。在外部函数中，本地作用域引用与全局作用域相同的名称空间：模块的名称空间。类定义在本地作用域中放置另一个名称空间。

意识到范围是由文本决定的：在模块中定义的函数的全局范围是该模块的名称空间，无论从何处调用函数或调用哪个别名。另一方面，名字的实际搜索是在运行时动态完成的 - 然而，语言定义正在向“编译”时间的静态名称解析发展，所以不要依赖动态名称解析！ （事实上​​，局部变量已经静态确定了。）

Python的一个特别的问题是，如果没有全局语句生效，对名称的分配总是进入最内层的范围。分配不会复制数据 - 它们只是将名称绑定到对象。删除也是如此：语句del x从本地作用域引用的名称空间中删除了x的绑定。实际上，所有引入新名称的操作都使用本地作用域：特别是import语句和函数定义将模块或函数名称绑定在本地作用域中。

全球声明可以用来表示特定的变量生活在全球范围内，应该在那里反弹;非本地声明表明特定的变量生活在一个封闭的范围内，应该在那里反弹。

#### 9.2.1 范围和命名空间示例
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

### 9.3 首先看类
类引入了一些新的语法，三种新的对象类型和一些新的语义。

#### 9.3.1 类定义语法
类定义最简单的形式如下所示：
```
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```
类定义，比如函数定义（def语句）必须在它们有效之前被执行。 （你可以想象将一个类定义放在一个if语句的分支中，或者放在一个函数中。

实际上，类定义中的语句通常是函数定义，但是其他语句是允许的，有时也是有用的 - 我们稍后再回来。一个类中的函数定义通常有一个特殊的参数列表形式，由方法的调用约定来决定 - 这也在后面解释。

当输入一个类定义时，会创建一个新的名称空间，并将其用作本地作用域 - 因此，所有对局部变量的赋值都将进入这个新的名称空间。特别是，函数定义在这里绑定新函数的名字。

当类定义保持正常（通过结束）时，创建一个类对象。这基本上是由类定义创建的命名空间内容的一个包装;我们将在下一节中学习更多有关类对象的知识。原来的本地作用域（在输入类定义之前生效的那个作用域）被恢复，类对象在这里被绑定到类定义头（在这个例子中是ClassName）给出的类名。

#### 9.3.2 类对象
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
#### 9.3.3 实例对象
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
#### 9.3.4 方法对象
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

#### 9.3.5 类和实例变量
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
正如“关于名称和对象的一个单词”中所讨论的，共享数据可能会涉及到涉及可变对象（如列表和字典）的令人惊讶的效果。 例如，以下代码中的技巧列表不应该用作类变量，因为只有一个列表将被所有Dog实例共享：
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
9.4。随机备注
数据属性覆盖具有相同名称的方法属性;为了避免意外的名称冲突，这可能会在大型程序中导致难以找到的错误，使用某种最小化冲突几率的约定是明智的。可能的约定包括大写方法名称，用小的唯一字符串（可能只是一个下划线）为数据属性名称加前缀，或者使用动词来表示数据属性的方法和名词。

数据属性可以由方法以及对象的普通用户（“客户”）引用。换句话说，类不可用于实现纯粹的抽象数据类型。事实上，Python中没有任何东西可以执行数据隐藏 - 这一切都是基于约定的。 （另一方面，用C编写的Python实现可以完全隐藏实现细节，并在必要时控制对对象的访问;这可以通过用C编写的Python扩展来使用）

客户应该谨慎使用数据属性 - 客户可能通过打印数据属性来混淆方法维护的不变量。请注意，客户端可以将自己的数据属性添加到实例对象中，而不会影响方法的有效性，只要避免名称冲突即可 - 命名规则再次可以在这里节省很多麻烦。

从方法中引用数据属性（或其他方法！）没有简写。我发现这实际上增加了方法的可读性：当通过方法浏览时，不会混淆局部变量和实例变量。

通常，方法的第一个参数叫做self。这只不过是一个惯例：名字自己对Python没有特别的意义。但是请注意，通过不遵循惯例，您的代码对其他Python程序员来说可能会不那么可读，同样可以想象，可能会编写一个依赖于这种约定的类浏览器程序。

任何作为类属性的函数对象都为该类的实例定义了一个方法。函数定义没有必要被文本包含在类定义中：将一个函数对象赋给类中的局部变量也是可以的。例如：
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
现在f，g和h都是类C的所有属性，它们都是指向函数对象的，因此它们都是C - h的实例的所有方法都与g完全等价。 请注意，这种做法通常只是混淆了程序的读者。

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
方法可以像普通函数一样引用全局名称。 与方法关联的全局范围是包含其定义的模块。 （一个类永远不会被用作全局范围）虽然在一个方法中很少有遇到使用全局数据的好理由，但是全局范围有许多合理的用途：一个是导入到全局范围的函数和模块 被方法使用，以及在其中定义的函数和类。 通常情况下，包含该方法的类本身是在这个全局范围内定义的，在下一节中，我们将找到一些很好的理由来说明一个方法想要引用它自己的类。

每个值都是一个对象，因此有一个类（也称为它的类型）。 它存储为对象.__ class__。

### 9.5 遗产
当然，如果不支持继承，语言特性就不值得称为“类”。 派生类定义的语法如下所示：
```
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```
名称BaseClassName必须在包含派生类定义的作用域中定义。 其他任意表达式也可以代替基类名称。 例如，当在另一个模块中定义基类时，这可能很有用：
```
class DerivedClassName(modname.BaseClassName):
```
派生类定义的执行过程与基类相同。当构造类对象时，基类将被记住。这用于解析属性引用：如果在类中未找到请求的属性，则搜索继续查找基类。如果基类本身是从其他类派生的，则递归地应用此规则。

派生类的实例化没有什么特别的：DerivedClassName（）创建了一个新的类实例。方法引用解析如下：如果需要，搜索相应的类属性，沿着基类的链下降，如果产生一个函数对象，则方法引用是有效的。

派生类可以重写它们的基类的方法。由于调用同一对象的其他方法时，方法没有特殊的权限，因此调用另一个在同一基类中定义的方法的基类方法可能最终会调用派生类的方法来覆盖它。 （对于C ++程序员来说：Python中的所有方法都是有效的）。

派生类中的重写方法事实上可能需要扩展而不是简单地替换同名的基类方法。有一个简单的方法直接调用基类方法：只需调用BaseClassName.methodname（self，arguments）。这对客户也是偶尔有用的。 （注意，这只有在基类可以在全局范围内作为BaseClassName访问时才有效。）

Python有两个与继承一起工作的内置函数：
- 使用isinstance（）来检查实例的类型：isinstance（obj，int）只有在obj .__ class__是int或从int派生的某个类时才为真。
- 使用issubclass（）来检查类继承：由于bool是int的子类，因此issubclass（bool，int）为True。 然而，issubclass（float，int）是False，因为float不是int的子类。

#### 9.5.1 多重继承
Python也支持多重继承的形式。 具有多个基类的类定义如下所示：
```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```
对于大多数用途来说，在最简单的情况下，您可以将从父类继承的属性视为深度优先，从左到右搜索，而不是在层次结构中存在重叠的相同类中搜索两次。因此，如果在DerivedClassName中找不到属性，则在Base1中搜索它，然后（递归地）在Base1的基类中搜索该属性，如果未在其中找到，则在Base2中搜索该属性，依此类推。

事实上，这比它稍微复杂一点，方法解析顺序动态改变以支持对super（）的合作调用。这种方法在一些其他多继承语言中被称为call-next-method，比单继承语言中的超级调用更强大。

动态排序是必要的，因为多重继承的所有情况都表现出一个或多个菱形关系（至少有一个父类可以通过最底层的多个路径访问）。例如，所有的类都从对象继承，所以任何多重继承的情况都提供了多于一条路径来达到对象。为了保持基类不止一次被访问，动态算法使搜索顺序线性化，保留每个类中指定的从左到右的顺序，每个父类只调用一次，这是单调的（意思是一个类可以被子类化而不影响父类的优先顺序）。综合起来，这些属性使得设计具有多重继承的可靠和可扩展的类成为可能。有关更多详细信息，请参阅https://www.python.org/download/releases/2.3/mro/。
### 9.6 私有变量
Python中不存在“私有”实例变量，除了在对象中不能访问的变量。但是，大多数Python代码都有一个约定：以下划线（例如_spam）作为前缀的名称应被视为API的非公开部分（无论是函数，方法还是数据成员） 。这应该被视为实施细节，如有更改，恕不另行通知。

由于类私有成员有一个有效的用例（即为了避免名称与由子类定义的名称发生冲突），对这种称为名称修改的机制的支持是有限的。任何形式的__spam标识符（至少两个前导下划线，最多一个尾随下划线）被文本替换为_classname__spam，其中classname是当前类名，前导下划线被去除。只要发生在类的定义内，就不用考虑标识符的语法位置。

名称修改有助于让子类重写方法而不会破坏intraclass方法调用。例如：
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
请注意，规则的设计主要是为了避免事故; 仍然可以访问或修改被认为是私有的变量。 这在特殊情况下甚至是有用的，比如在调试器中。

请注意，传递给exec（）或eval（）的代码并不认为调用类的类名是当前类; 这与全局语句的效果相似，其效果同样局限于字节编译在一起的代码。 getattr（），setattr（）和delattr（）以及直接引用__dict__的时候也是一样的限制。

### 9.7 什物(Odds and Ends)
有时候，有一个类似于Pascal“记录”或C“结构”的数据类型是有用的，它将几个命名的数据项捆绑在一起。 一个空的类定义将很好地做到：
```
class Employee:
    pass

john = Employee()  # Create an empty employee record

# Fill the fields of the record
john.name = 'John Doe'
john.dept = 'computer lab'
john.salary = 1000
```
一段期望特定抽象数据类型的Python代码通常可以传递一个模拟该数据类型方法的类。 例如，如果你有一个从文件对象中格式化数据的函数，你可以用一个方法read（）和readline（）来定义一个类，它从字符串缓冲区中获取数据，并将其作为参数传递。

实例方法对象也有属性：m .__ self__是m（）方法的实例对象，m .__ func__是方法对应的函数对象。

### 9.8 迭代器
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
这种访问方式清晰，简洁，方便。 迭代器的使用遍历和统一Python。 在幕后，for语句在容器对象上调用iter（）。 该函数返回一个迭代器对象，该对象定义一次访问容器中元素的方法__next __（）。 当没有更多的元素时，__next __（）引发一个StopIteration异常，它告诉for循环终止。 您可以使用next（）内置函数调用__next __（）方法; 这个例子展示了它是如何工作的：
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
看到了迭代器协议后面的机制，很容易将迭代器行为添加到类中。 定义一个用__next __（）方法返回对象的__iter __（）方法。 如果类定义了__next __（），那么__iter __（）可以返回self：
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

### 9.9 生成器
生成器是创建迭代器的简单而强大的工具。 它们像常规函数一样编写，但是只要他们想返回数据就使用yield语句。 每当next（）被调用时，生成器会从停止的地方恢复（它会记住所有的数据值以及上次执行的语句）。 一个例子表明，生成器可以很容易地创建：
```
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
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

### 9.10 生成器表达式
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
脚注

[1]除了一件事。 模块对象有一个叫做__dict__的秘密只读属性，它返回用于实现模块名称空间的字典; 名称__dict__是一个属性，但不是全局名称。 很明显，使用这样做违反了命名空间实现的抽象，应该被限制在事后调试器之类的东西。

### python关于类的总结

- 比如必须调用父类的构造方法__init__才能正确初始化父类实例属性，使得子类实例对象能够继承到父类实例对象的实例属性；
- 再如需要重写父类方法时，有时候没有必要完全摒弃父类实现，只是在父类实现前后加一些实现，最终还是要调用父类方法

1、 ===
f = F()
a = F().a
这一语句，会执行F()类的__init__()函数。但不会执行F()的父类的__init__()，必须显示地调用才行。

2、===
f = F()
子类会继承父类的 “类变量” ，如果子类改变了这些变量的值，会影响子类的这个变量的值。
如果父类的变量，是通过__init__()定义的，则在子类中，不会继承，除非子类显示地执行了父类的__init__()。

3、===
在子类中，如果 F()是A()的子类， 在F()中通过 A.__init__(self)调用初始化方法，这里面的self，代表F()，而不是A()。

具体的代码如下：
```
riddle@:~/gott/pt/class$ cat t1.py
class A(object):
 a = "--a"
 def __init__(self):
   self.aa = "aa"
   print("enter A")
   print("leave A")

class B(object):
 def __init__(self):
   self.bb = "bb"
   print("enter B")
   print("leave B")

class C(A):
 def __init__(self):
   self.cc = "cc"
   print("enter C")
   super(C, self).__init__()
   print("leave C")

class D(A):
 def __init__(self):
   self.dd = "dd"
   print("enter D")
   super(D, self).__init__()
   print("leave D")

class E(B, C):
 a = "--e"
 def __init__(self):
   self.ee = "ee"
   print("??",self.ff)
   self.ff = "e.ff"
   print("enter E")
   B.__init__(self)
   C.__init__(self)
   print("leave E")

class F(E, D):
 def __init__(self):
   self.ff = "f.ff"   # 如果没有这一句，那么 class E中的 print("??",self.ff)会报错。
   print("enter F")
   E.__init__(self)
   D.__init__(self)
   print("leave F")

f=F()
print(f.ff)
#print(f.dd)
#print(f.aa)
print(f.a)
print(F().ff)
print(F().a)
#print(F().aa)
#print "MRO:", [x.__name__ for x in E.__mro__]
riddle@:~/gott/pt/class$
```
##### Python's Super is nifty, but you can't use it（Python's Super Considered Harmful）
https://fuhm.net/super-harmful/

##### MRO & super
http://blog.csdn.net/seizef/article/details/5310107
##### Python进阶-继承中的MRO与super
http://blog.csdn.net/SakuraInLuoJia/article/details/73916878
