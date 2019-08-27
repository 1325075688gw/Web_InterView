### 描述

**super()** 函数是用于调用父类(超类)的一个方法。

super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。

MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。

### 语法

以下是 super() 方法的语法:

```
super(type[, object-or-type])
```

### 参数

- type -- 类。
- object-or-type -- 类，一般是 self

Python3.x 和 Python2.x 的一个区别是: Python 3 可以使用直接使用 super().xxx 代替 super(Class, self).xxx :

#### Python3.x 实例：

```python
class A:
     def add(self, x):
         y = x+1
         print(y)
class B(A):
    def add(self, x):
        super().add(x)
b = B()
b.add(2)  # 3
```



#### Python2.x 实例：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class A(object):   # Python2.x 记得继承 object
    def add(self, x):
         y = x+1
         print(y)
class B(A):
    def add(self, x):
        super(B, self).add(x)
b = B()
b.add(2)  # 3
```



**python子类调用父类成员有2种方法，分别是普通方法和super方法**

- 假设Base是基类

```python
class Base(object):
      def __init__(self):
           print “Base init”
```

- 则普通方法如下

```python
class Leaf(Base):
       def __init__(self):
              Base.__init__(self)
              print “Leaf init”

```

- super方法如下

```python
class Leaf(Base):
       def __init__(self):
              super(Leaf, self).__init__()
              print “Leaf init”
```

##### 在上面的简单场景下，两种方法的效果一致：

```python
class Foo(object):
    def __init__(self):
        self.name = 'Foo'
 
    def hi(self):
        print('hi,Foo')
 
class Foo2(Foo):
    def hi(self):
        print('hi,Foo2')
 
if __name__ == '__main__':
    f = Foo2()
    f.hi()    #默认调用子类方法

    
结果：

hi,Foo2
```



```python
class Foo(object):
    def __init__(self):
        self.name = 'Foo'
 
    def hi(self):
        print('hi,Foo')
 
class Foo2(Foo):
    def hi(self):
        #Foo.hi(self) #方法1：在重写的方法内强制调用 父类名.父类方法名() 如果需要参数，则需要传参
        super(Foo2,self).hi() #方法2： super(子类名，子类对象）.父类方法（） 如果需要参数，则需要传参
        #super().hi()  #方法3：仅支持python3
        print('hi,Foo2')
 
if __name__ == '__main__':
    f = Foo2()
    f.hi()    #调用父类方法

    
结果：

hi,Foo
hi,Foo2
```

调用父类的——属性

```python
class Foo(object):
    def __init__(self):
        self.name = 'Foo'
 
    def hi(self):
        print('hi,Foo')
 
class Foo2(Foo):
    def __init__(self):
        #Foo.__init__(self)  #方法1：在重写的方法内强制调用 父类名.父类方法名() 如果需要参数，则需要传参
        #super(Foo2,self).__init__()   #方法2： super(子类名，子类对象）.父类方法（） 如果需要参数，则需要传参
        super().__init__()    #方法3： 仅python3支持
 
    def hi(self):
        print('hi,Foo2')
 
if __name__ == '__main__':
    f = Foo2()
    f.hi()    #调用父类方法
    print(f.name)

    
    
结果：

hi,Foo2
Foo
```



## 2.   钻石继承遇到的难题

当我们来到钻石继承场景时，我们就遇到了一个难题：

如果我们还是使用普通方法调用父类成员，代码如下：

```python
class Base(object):
  def __init__(self):
    print “Base init”
class Medium1(Base):
  def __init__(self):
    Base.__init__(self)
    print “Medium1 init”
class Medium2(Base):
  def __init__(self):
    Base.__init__(self)
    print “Medium2 init”
class Leaf(Medium1, Medium2):
  def __init__(self):
    Medium1.__init__(self)
    Medium2.__init__(self)
    print “Leaf init”

```



```python
当我们生成Leaf对象时，结果如下：

>>> leaf = Leaf()
Base init
Medium1 init
Base init
Medium2 init
Leaf init
可以看到Base被初始化了 两次 ！这是由于Medium1和Medium2各自调用了Base的初始化函数导致的。
```

### 3.   各语言的解决方法

钻石继承中，父类被多次初始化是个非常难缠的问题，我们来看看其他各个语言是如何解决这个问题的：

#### 3.1. C++

C++使用虚拟继承来解决钻石继承问题。

Medium1和Medium2虚拟继承Base。当生成Leaf对象时，Medium1和Medium2并不会自动调用虚拟基类Base的构造函数，而需要由Leaf的构造函数显式调用Base的构造函数。

#### 3.2. Java

Java禁止使用多继承。

Java使用单继承+接口实现的方式来替代多继承，避免了钻石继承产生的各种问题。

#### 3.3. Ruby

Ruby禁止使用多继承。

Ruby和Java一样只支持单继承，但它对多继承的替代方式和Java不同。Ruby使用Mixin的方式来替代，在当前类中mixin入其他模块，来做到代码的组装效果。

#### 3.4. Python

Python和C++一样，支持多继承的语法。但Python的解决思路和C++完全不一样，Python是的用就是super

我们把第2章的钻石继承用super重写一下，看一下输出结果

```python
class Base(object):
  def __init__(self):
    print “Base init”
class Medium1(Base):
  def __init__(self):
    super(Medium1, self).__init__()
    print “Medium1 init”
class Medium2(Base):
  def __init__(self):
    super(Medium2, self).__init__()
    print “Medium2 init”
class Leaf(Medium1, Medium2):
  def __init__(self):
    super(Leaf, self).__init__()
    print “Leaf init”

```



```python
我们生成Leaf对象：

>>> leaf = Leaf()
Base init
Medium2 init
Medium1 init
Leaf init

可以看到整个初始化过程符合我们的预期，Base只被初始化了1次。而且重要的是，相比原来的普通写法，super方法并没有写额外的代码，也没有引入额外的概念
```

#### 4.   super的内核：mro

```python
>>> Leaf.mro()

[Leaf, Medium1, Medium2, Base]
```

```python
可以看到mro方法返回的是一个祖先类的列表。Leaf的每个祖先都在其中出现一次，这也是super在父类中查找成员的顺序。

通过mro，python巧妙地将多继承的图结构，转变为list的顺序结构。super在继承体系中向上的查找过程，变成了在mro中向右的线性查找过程，任何类都只会被处理一次。

通过这个方法，python解决了多继承中的2大难题：

1. 查找顺序问题。从Leaf的mro顺序可以看出，如果Leaf类通过super来访问父类成员，那么Medium1的成员会在Medium2之前被首先访问到。如果Medium1和Medium2都没有找到，最后再到Base中查找。

2. 钻石继承的多次初始化问题。在mro的list中，Base类只出现了一次。事实上任何类都只会在mro list中出现一次。这就确保了super向上调用的过程中，任何祖先类的方法都只会被执行一次。
```

#### 5.1. super(type, obj)

当我们在Leaf的__init__中写这样的super时：

```python
class Leaf(Medium1, Medium2):
       def __init__(self):
              super(Leaf, self).__init__()
              print “Leaf init”

```

super(Leaf, self).__init__()的意思是说：

1. 获取self所属类的mro, 也就是[Leaf, Medium1, Medium2, Base]
2. 从mro中Leaf右边的一个类开始，依次寻找__init__函数。这里是从Medium1开始寻找
3. 一旦找到，就把找到的__init__函数绑定到self对象，并返回

从这个执行流程可以看到，如果我们不想调用Medium1的__init__，而想要调用Medium2的__init__，那么super应该写成：super(Medium1, self)__init__()

#### 5.2. super(type, type2)

当我们在Leaf中写类方法的super时：

```python
class Leaf(Medium1, Medium2):
       def __new__(cls):
              obj = super(Leaf, cls).__new__(cls)
              print “Leaf new”
              return obj

```



super(Leaf, cls).__new__(cls)的意思是说：

1. 获取cls这个类的mro，这里也是[Leaf, Medium1, Medium2, Base]
2. 从mro中Leaf右边的一个类开始，依次寻找__new__函数
3. 一旦找到，就返回“ 非绑定 ”的__new__函数

由于返回的是非绑定的函数对象，因此调用时不能省略函数的第一个参数。这也是这里调用__new__时，需要传入参数cls的原因

同样的，如果我们想从某个mro的某个位置开始查找，只需要修改super的第一个参数就行



#### 实战

```python
# 作者     ：gw
# 创建日期 ：2019-08-27  下午 17:08
# 文件名   ：mro.py

class A():
    number = 10

    def __init__(self):
        print('enter A')
        print('leave A')

    def run(self):
        print('怡鸣可以跑的很快!')

    @classmethod
    def eat(cls, a):
        print('怡鸣快来吃饭了!')
        print(cls,'----')
        print(a)


class B(A):
    def __init__(self):
        print('enter B')
        super().__init__()
        print('leave B')

    def run(self):
        super(B, self).run()
        print("我来了")
        super().eat('sf')


    @classmethod
    def eat(cls):
        # super().eat('我在这')
        super(B, cls).eat('我在这')
        print(super().number)


class C(A):
    def __init__(self):
        print('enter C')
        super().__init__()
        print('leave C')


class D(B, C):
    def __init__(self):
        print('enter D')
        super().__init__()
        print('leave D')


d = D()
print(D.mro())
b = B()
b.run()
B.eat()
```



```python
F:\soft\Anaconda3\python.exe D:/code/python_code/dailyfresh_now/mro.py
enter D
enter B
enter C
enter A
leave A
leave C
leave B
leave D
[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
enter B
enter A
leave A
leave B
怡鸣可以跑的很快!
我来了
怡鸣快来吃饭了!
<class '__main__.B'> ----
sf
怡鸣快来吃饭了!
<class '__main__.B'> ----
我在这
10

Process finished with exit code 0

```

