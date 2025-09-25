# 面向对象的编程  

在python中，所有东西都是对象
- 类（class）：定义对象的属性（数据）和方法（功能）
- 对象（object）：类的实例

---

## 封装

**类的基本结构：**      
- `class`：关键字，用于定义新类
- `__init__`：类的初始化方法，在创建类的实例时自动被调用，用于为对象的属性赋初始值
- `self`：类方法的第一个参数，代表实例本身，通过self可以在方法内部访问或修改实例的属性
- `实例属性`：通过 `self.attribute_name`定义的属性，每个属性都有自己独立的属性副本  

```python
class Dog:
	def __init__(self,name,age):
		self.name = name
		self.age = age
		self.is_hungry = False
	def bark(self):
		print(f"{self.name}：汪汪！")
		self.is_hungry = True
```

python的封装哲学与其他语言不同
- 没有 `private`等关键字，所有属性和方法都是默认公开的
- python的封装基于一种约定，通过命名来表明一个属性或方法是用于内部使用

1. 公有（public）：默认的，没有前缀
2. 保护（protected）：在属性或方法前加一个单下滑线 `_`，这是一个约定，告诉其他开发者不要在类的外部直接访问或修改它

```python
class BankAccount:
	def __init__(self,initial_balance):
		self._balance = initial_balance
	def get_balance(self):
		return self._balabce

account = BanKAccount(100)
print(account._balance)  # 可以访问，但约定上不推荐
```

3. 私有（private）：在属性或方法名前加双下划线 `__`这是一种特殊的机制，并不是真正的私有化，而是python对属性名进行的名称修饰，python解释器会自动将 `__name`重命名为 `_ClassName__name`，使得在外部无法直接访问

```python
class BankAccount:
	def __init__(self,initial_balance):
		self.__banlance = initial_balance
	def get_balance(self):
		return self.__balance
		
account = BankAccount(100)
print(account.__balance)  # 这段代码会引发AttributeError
print(account.get_balance()) # 必须通过公共方法访问
print(account._BankAccount__balance) # 理论上可行，但不应该这样做
```

此外，类中还有许多特殊方法，在python中又称魔术方法（magic methods）或者Dunder方法（Double Underscore，双下划线的缩写），这是python中独特和强大的功能   

能够让自定义对象像内置类型（如字符串、列表）一样工作，从而与python的内置函数和运算符无缝协作   

- 对象表示
	- `__str__(self)`：返回对象非正式，可读的字符串表示，print( )函数会优先调用它

```python
class Point:
	def __init__(self,x,y):
		self.x = x
		self.y = y
	def __str__(self):
		return f"坐标点({self.x},{self.y})"
		
p = Point(1,2)
print(p)
```

- 运算符重载
	- `__eq__(self,other)`：定义想等性
	- `__lt__(self,other)`：定义小于
	- `__add__(self,other)`：定义加法
	- `__len__(self)`：定义len( )函数的行为

在python中，我们一般约定other代表另一个对象

```python
class Vector:
	def __init__(self,x,y):
		self.x = x
		self.y = y
	def __add__(self,other):
		return Vector(selx.x + other.x,self.y + self.y)
	def __eq__(self,other):
		return self.x == other.x and self.y ==other.y
	def __str__(self):
		return f"({self.x},{self.y})"

v1 = Vector(1,2)
v2 = Vector(3,4)
print(f"向量加法：{v1+v2}")
if v1==v2:
	print(f"v1 = v2")
```

- 容器行为
	- `__getitem__(self,key)`：定义获取元素时的行为（obj\[key\]）
	- `__setitem__(self,kry,value)`：定义设置元素时的行为（obj\[key\]=value）
	- `__iter__(self)`：定义迭代器的获取方式（for循环会自动调用）

---

## 继承

继承允许一个类（称为子类或派生类）从另一个类（称为父类、基类或者超类）继承属性和方法    

在定义子类时，只需将父类的名字放在圆括号里  

```python
class Animal:
	def __init__(self,name):
		self.name = name
	def speak(self):
		print("动物发出声音。")

class Dog(Animal):
	def __init__(self,name,breed):
		# 调用父类的__init__方法，确保父类属性初始化
		super().__init__(name)
		self.breed = breed  # 子类特有属性
	# 方法重载
	# 子类可以提供自己的speak方法，覆盖父类的方法
	def speak(self):
		print(f"{self.name}说：汪汪！")

my_dog = Dog("papa","金毛")
my_dog.speak()  # 调用的是子类重载的speak方法
print(my_dog.name()) # 继承自父类的name属性
```

但格外需要注意的是，如果在子类中定义了`__init__`方法，它会覆盖父类的`__init__`，这意味着父类的初始化逻辑将不会执行，因此必须在子类的`__init__`方法中显式的调用父类的`__init__`   

`super()`函数是一个非常重要函数，它能够动态的调用MRO（方法解析顺序，Method Resolution Order）链中下一类的方法   

在python中没有虚继承的概念，因为MRO机制从根本上解决了这个问题

在初始化问题上，如果不使用`super()`，而是使用`ParentClass.__init__()`，容易导致父类的 `__init__`重复调用，从而引发逻辑错误或不必要的开销  

以菱形继承结构为例：

```python
class A:
	def __init__(self):
		print("A.__init__ called.")

class B(A):
	def __init__(self):
		print("B.__init__ called.")
		A.__init__(self)

class C(A):
	def __init__(self):
		print("C.__init__ called.")
		A.__init__(self)
		
class D(B,C):
	def __init__(self):
		print("D.__init__ called.")
		B.__init__(self)
		C.__init__(self)

d = D()
# 输出
# D.__init__ called. 
# B.__init__ called.
# A.__init__ called.
# C.__init__ called.
# A.__init__ called.  # 这里A的初始化函数调用了两次！
```

如果使用 `super()`话：

```python
class A:
    def __init__(self):
        print("A.__init__ called.")
        
class B(A):
    def __init__(self):
        print("B.__init__ called.")
        super().__init__() # 动态调用下一个父类的方法

class C(A):
    def __init__(self):
        print("C.__init__ called.")
        super().__init__()

class D(B, C):
    def __init__(self):
        print("D.__init__ called.")
        super().__init__() # 动态调用下一个父类的方法

d = D()
# 输出:
# D.__init__ called.
# B.__init__ called.
# C.__init__ called.
# A.__init__ called.
```

这个结果是正确的，每个 `__init__`被调用了一次， `super()`按照D的MRO顺序（`D->B->C->A`）逐一调用 `__init__`方法

并且在多重继承的情况下，如果不同父类含有相同的方法，使用 `super()`会按照MRO顺序分别调用一次

```python
class A:
    def greet(self):
        print("Hello from A")
        super().greet()

class B(A):
    def greet(self):
        print("Hello from B")
        super().greet()

class C(A):
    def greet(self):
        print("Hello from C")
        super().greet()

class D(B, C):
    def greet(self):
        print("Hello from D")
        super().greet()

# 查看 D 的方法解析顺序
print("D 的方法解析顺序 (MRO):", D.mro())
print("-" * 30)

# 调用 D 实例的 greet 方法
d = D()
d.greet()
```

输出结果：

```
D 的方法解析顺序 (MRO): [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
------------------------------ 
Hello from D      # D.greet() 
Hello from B      # super() 在 D 中调用 B.greet() 
Hello from C      # super() 在 B 中调用 C.greet() 
Hello from A      # super() 在 C 中调用 A.greet()
```

---

## 多态

多态指的是不同类的对象可以对同一个方法调用做出不同的响应  

```python
class Animal:
	def speak(self):
		raise NotImplementedError("子类必须实现speak方法")  

class Dog(Animal):
	def speak(self):
		return "汪汪！"

class Cat(Animal):
	def speak(self):
		return "喵喵～"

def animal_sound(animal):
	"""
	这个函数能够处理任何实现了speak()方法的对象
	它不关心对象的具体类型
	"""
	print(animal.speak())
# 创建不同类型的对象
dog = Dog()
cat = Cat()
# 同一个函数，传入不同类型的对象，得到不同的结果
print("------")
animal_sound(dog)  # 输出：汪汪！
animal_sound(cat)  # 输出：喵喵～
```
