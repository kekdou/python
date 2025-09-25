# 基本语法

python中用缩进来表示代码的层次结构，并且每个语句不需要分号，这是python的美观哲学，因此在写python过程中不能胡乱缩进

---

## 定义变量

- 在python中不需要提前声明数据类型，只需要给变量一个名字并直接赋值，python会自动识别数据类型

---

## 控制流

---

### 条件语句

- `if`
- `elif`
- `else`

注意每个条件语句的末尾必须有冒号，并且其下代码块必须缩进  

```python
age = 30
status = ""
if age >= 18:
	status = "成年"
else:
	status = "未成年"
print(status)
```

三元运算符：对于简单的 `if-else`表达式，可以用更加紧凑的三元运算符  

```python
status = "成年" if age >= 18 else "未成年"
```

此外，条件语句常用 `in`运算符来检查某个元素是否在序列中

```python
fruits = ["apple","banana"]
if "apple" in fruits:
	print("小苹果！")
```

*再次提醒注意冒号和缩进！*  

---

### 循环

python中主要有 `for`和 `while`循环，并没有 `do...while`  

**for：**  

for可以遍历任何可迭代对象（列表，字符串，元组等）中的元素  

`for item in iterable:`  

```python
my_string = "Pyhon"
for char in my_string:
	print(char)
# 输出:   
# P 
# y 
# t 
# h 
# o 
# n
```

需要注意的是，在for char中相当于同时给char进行了声明和赋值，而无需提前声明    

此外对于整数序列的遍历，常用 `range()`函数 ：  
- `range(stop)`：从0到stop-1
- `range(start,stop)`：从start到stop-1
- `range(start,stop,step)`：从start到stop-1，步长为step  

```python
for i in range(3):
	print(i,end="")
# 输出：0，1，2
```

为了写出简洁美观的代码，可以灵活运用推导式    

`[expression for item in iterable if condition]`：
- `expression`：想对item执行的操作
- `item`：可迭代对象中的每个元素
- `iterable`：想遍历的序列（列表，字符串，range( )等）
- `if condition`：可选部分，用于过滤元素

```python
# 创建一个包含1到10的平方数列表
# 传统for循环
squares = []
for i in range(1,11):
	squares.append(i*i)
# 列表推导式
squares_comp = [i*i for i in range(1,11)]
```

此外，在有序容器中，如果需要在遍历时同时获取索引和元素，常用 `enumerate()`函数

```python
fruits = ["apple","banana","cherry"]
for index,fruit in enumerate(fruits):
	print(f"索引{index}：{fruit}") 
```

进一步，如果需要同时遍历两个或多个列表时，可以使用 `zip()`函数

```python
names = ["Alice","Bob"]
ages = [25,30]
for name,age in zip(names,ages):
	print(f"{name}今年{age}岁。")
```

**while：**   

- `break`：立即跳出循环
- `continue`：跳过当前循环的剩余部分，直接进入下一次循环

```python
count = 0
while count < 10:
	if count == 5:
		count += 2
		continue
	if count == 8:
		break
	print(count,end=" ")
	count +=1
```

---

### 迭代

`iter()`函数是python迭代机制的基石，可以手动从一个可迭代对象（iterable）中获取迭代器（iterator），实现更精细的控制  

但不同于C++，python中的迭代器是单向的，且只能使用next( )来获取下一个元素，其底层是一个简单对象，实现了 `__iter__`和`__next__`方法    

```python
my_list = [1,2,3]
iterator = iter(my_list)
print(next(iterator))  # 输出：1
print(next(iterator))  # 输出：2
# 当迭代器耗尽后便无法恢复
```

此外iter( )还有一个更高级的用法：哨兵值迭代，iter( )接收两个参数，一个可调用的对象（例如一个函数），和一个哨兵值

`iter(callable,sentinel)`会创建一个迭代器，不断调用 `callable()`，直到返回值等于 `sentinel`时停止   

```python
with open('large_file.txt','r') as file:
	# iter()会不断调用f.read(1024)
	# 直到返回值等于''时停止，即为空时
	for chunk in iter(lambda: f.read(1024),''):
		print(f"读取到{len(chunk)}个文字。")
```

当一个函数包含 `yield`语句时，它就变成了一个生成器函数，调用这个函数时，它不会立即执行函数体内的代码，而是返回一个生成器对象   

生成器是一种特殊的迭代器，它不会一次性将所有数据加载到内存中，而是在每次迭代时按需生成一个值，在内存首受限的情况下十分高效  

理解 `yield`的最好方式是将其视作一个特殊的 `return`，但它返回一个生成器对象，而不是立即返回一个值并结束函数

```python
# 一个生成器函数，按需生成斐波那契数列
def fibonacci():
	a,b = 0,1
	while 1:
		yield a
		a,b = b,a+b
# 可以只取出前5个数，而不会生成无限个
fib_gen = fibonacci()
for i in range(5):
	print(next(fib_gen),end=" ")
# 输出：0 1 1 2 3
```

---

## 函数

python中函数不需要写返回值类型，只需加上 `def`关键字即可创建函数

如果在函数内部没有使用 `return`语句，会隐式的返回 `None`  

同样，python中函数也是局部作用域，内部定义的变量只在内部可见  

---

**基本用法：**   

```python
def add(a,b):
	return a + b
	
result = add(5,3)
```

在更加良好习惯下的定义函数，可以使用三引号编写文档字符串，来解释函数的作用、参数和返回值    

```python
def get_full_name(first_name,last_name):
	"""
	拼接姓和名，返回完整的名字
	Args：
		first_name：名
		last_name：姓
	Return:
		返回完整的名字字符串
	"""
	return f"{first_name}{last_name}"
```

**参数类型**：  

python中参数类型非常灵活，可分为四种
- `位置参数`：最基本的参数，在调用函数时，传入参数顺序必须与函数定义时的顺序一样   

```python
def describe_pet(animal,name):
	print(f"我的宠物是一只{animal}，它的名字是{name}")

describe_pet("dog","papa")
# 必须按顺序传入
```

- `关键字参数`：在调用函数时，可以通过参数的名称来赋值。好处是参数顺序更不重要，代码意图更加明显

```python
def create_user(name,age,city):
	print(f"创建用户：{name}，年龄{age}，城市{city}")

create_user(age=25,name="kekdou",city="Peking")
# 使用关键字参数，顺序可以颠倒
```

- `默认参数`：定义函数时设置缺省值   

python中参数默认值规则与C++类似，所有带有默认值的参数都必须放在没有默认值的参数之后   

但需要注意可变参数的陷阱，在python中，函数的默认参数是在函数被定义时（即加载到内存时）初始化的，而不是在每次函数被调用时  

如果默认参数是一个可变对象，那么所有对该函数的调用都会共享一个对象  

```python
def add_item(item,list=[1,2]):
	list.append(item)
	return list
	
list1 = add_item(3)
print(f"第一次调用：{list1}")
list2 = add_item(4)
print(f"第二次调用：{list2}")
# 第一次调用：[1,2,3]
# 第二次调用：[1,2,3,4]
```

尽管好像不太常用(

- `可变参数`：
	- `*args`：用于接收任意数量的位置参数，这些参数会打包成一个元组
	- `**kwargs`：用于接收任意数量的关键字参数，这些参数会打包成一个字典

```python
def print_info(name,*subjects,**grades):
	print(f"学生姓名：{name}")
	print(f"所选课程：{subjects}")
	print("各科成绩：")
	for subject,grade in grades.items():
		print(f"{subject}:{grade}")
		
print_info("kekdou","math","English",english=95,chemistry=88)
```

千万不要认为* 是指针，在python中没有指针的概念

**函数名与重定义：**   

python中不支持函数重载，即不能有多个同名函数但参数列表不同。后定义的函数会覆盖先定义的函数   

**参数传递：**  

python的参数传递称为传对象引用
- 对于不可变对象：int、str、tuple等，在函数内部修改它们时，实际是创建了一个新的对象，并让局部变量指向这个新对象。原对象不受影响，类似传值
- 对于可变对象：list、dict、set等，由于函数内部和外部的变量指向同一个对象，因此在函数内部对该对象的任何 **原地修改**都会反映到外部变量上  

用一个实例来理解：  

```python
def my_func(my_list):
	my_list.append(4)
original_list = [1,2,3]
my_func(original_list)
```

1. `original_list = [1,2,3]`
	- 在内存中创建了一个列表对象 `[1,2,3]`
	- 变量 `original_list`成了一个引用，指向这个列表对象的内存地址

```
original_list ---> [1,2,3]
```

2. `my_func(original_list)`调用
	- python创建了一个新的局部变量`my_list`，作为参数
	- `my_list` 接收了 `original_list`引用的副本
	- 这意味着`my_list`也指向了和`original_list` 同一个列表对象

```
original_list ---> [1,2,3]
						<--- my_list(局部变量)
```

3. 函数内部执行 `my_list.append(4)`
	-  python找到`my_list`指向的那个列表对象，并在它的末尾添加了4
	- 内存中的列表对象现在变成了`[1, 2, 3, 4]`

```
original_list ---> [1,2,3，4]
						<--- my_list(局部变量)
```

4. 函数执行完毕
	- 局部变量 `my_list` 被销毁
	- 但它修改过的那个列表对象仍然存在，并且 `original_list` 仍然指向它

再看另一个实例：   

```python
def my_func(my_list):
	my_list = [4,5,6]
original_list = [1,2,3]
my_func(original_list)
```

1. 同上
2. 同上
3. 执行`my_list = [4, 5, 6]`
	- python 在内存中**新创建**了一个列表对象 `[4, 5, 6]`
	- 局部变量 `my_list` 的引用被**重新赋值**，让它指向这个新的对象
	- `original_list` 的引用**没有改变**，它仍然指向旧的 `[1, 2, 3]`

```
original_list ---> [1, 2, 3]
my_list ---> [4, 5, 6]
```

4. 函数执行完毕
	- 局部变量 `my_list` 被销毁
	- `original_list` 仍然指向`[1, 2, 3]`，所以外部变量没有被改变

因此如果要修改外部变量的值，应该使用return语句，如果想让函数返回多个值，可以return一个元组，再使用元组拆包的方式将这些值重新赋给外部变量   

```python
def process_data(num,text):
	"""
	返回一个新的数字和新的字符串
	"""
	new_num = num + 10
	new_text = "Modified:" + text
	return new_num,new_text

x = 5
y = "Original"
x,y = process_data(x,y)
```

