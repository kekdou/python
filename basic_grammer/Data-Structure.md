# 数据结构

---

## 数据基本类型  

Python中有四种最基本的数据类型，可以使用`type()`获得数据类型   

---

### int & float & bool

python中int没有大小限制，可以表示任意大的整数   

并且python没有独立的double类型，float就是双精度浮点数，使用64位来储存，提供15-17位十进制有效数字的精度

尽管python中float精度很高，但由于内部是二进制储存的问题，某些十进制小数（0.1，0.2）无法被精确表示，导致著名的精度问题

```python
result = 0.1 + 0.2
print(result)
# 输出：0.30000000000000004
```

如果需要更精确的计算，需要使用`decimal`模块

数值类型自身的方法很少，一般与一些内置函数一起使用  

- `abs(number)`：返回绝对值
- `round(number,ndigits)`：对浮点数进行四舍五入  

```python
x = -15
absolute_x = abs(x)
print(f"{x}的绝对值是：{absolute_x}")  # -15的绝对值是：15

pi = 3.14159
rounded_pi = round(pi,2)
print(f"圆周率四舍五入后：{rounded_pi}")  # 圆周率四舍五入后：3.14

float_num = 10.8
int_num = int(float_num)
print(f"{float_num}转换为整数:{int_num}") # 输出: 10.8 转换为整数: 10  

int_val = 15
float_val = float(int_val)
print(f"{int_val}转换为浮点数:{float_val}") # 输出: 15 转换为浮点数: 15.0
```

---

### str

在python中没有独立的char类型，单个字符视为长度为1的字符串类型   

字符串一旦创建，就不能被修改。所有对字符串的操作（如替换、拼接）都会生成一个新的字符串对象

并且字符串是有序的，可以通过索引访问字符串中的每个字符  

```python
my_string = "Hello"
# 通过索引获取第一个字符
first_char = my_string[0]

print(f"first_char 的值是: {first_char}")
print(f"first_char 的类型是: {type(first_char)}")
print(f"first_char 的长度是: {len(first_char)}")

# 输出结果：
# first_char 的值是: H 
# first_char 的类型是: <class 'str'> 
# first_char 的长度是: 1
```

常用方法：
- `str.lower()/str.upper()`：转小写/转大写
- `str.strip()/str.lstrip()`：移除两断空白/移除左端空白
- `str.split(separator)`：按分隔符切片，返回list
- `str.join(list)`：用字符串连接列表元素，返回str
- `str.find(sub)`：查找子串位置
- `str.replace(old,new)`：替换子串

```python
text = "  Hello, world!  "
# 1. 移除首尾空白
stripped_text = text.strip()
print(f"'{stripped_text}'") # 输出: 'Hello,world!'

# 2. 转换大小写
upper_text = stripped_text.upper()
lower_text = stripped_text.lower()
print(f"大写: {upper_text}") # 输出: 大写: HELLO, WORLD!
print(f"小写: {lower_text}") # 输出: 小写: hello, world!

# 3. 检查开头和结尾
starts_with_H = stripped_text.startswith("H")
ends_with_d = stripped_text.endswith("d!")
print(f"以'H'开头吗? {starts_with_H}") # 输出: 以'H'开头吗? True
print(f"以'd!'结尾吗? {ends_with_d}") # 输出: 以'd!'结尾吗? True

# 4. 查找和替换
index = stripped_text.find("world")
replaced_text = stripped_text.replace("world", "Python")
print(f"'world'的位置: {index}") # 输出: 'world'的位置: 7
print(f"替换后的文本: {replaced_text}") # 输出: 替换后的文本: Hello, Python!

# 5. 分割和连接
words = stripped_text.split(" ")
joined_words = "-".join(words)
print(f"分割成列表: {words}") # 输出: 分割成列表: ['Hello,', 'world!']
print(f"连接成字符串: {joined_words}") # 输出: 连接成字符串: Hello,-world!
```

值得注意的是，split( )方法默认是根据任何空白字符（如空格，制表符，换行符等）进行分隔，而不是分隔成单个字符  

```python
sentence = "  Python  is a powerful language.  "
words = sentence.split()
print(words)
# 输出: ['Python', 'is', 'a', 'powerful', 'language.']
```

想要分隔成单个字符，需要传入一个空字符串`''`作为分隔符  

```python
name = "Hello"
characters = name.split('')
print(characters)
# 输出: ['H', 'e', 'l', 'l', 'o']
```

此外，对与字符串的拼接，如果在循环中频繁使用`+=`会产生大量临时字符串对象，导致性能下降，推荐使用`"".join(list)`

---

## 数据结构

---

### list

list是python中最常用和重要的数据结构之一  
- 有序：列表中元素有固定的顺序，可以通过索引（从0开始）访问任何元素
- 可变：列表是可变对象，随时可以在原地修改，添加或删除元素，而无需创建一个新的列表
- 异构：列表可以包含不同数据类型的元素
- 允许重复：列表可以包含多个相同的元素

**基本用法：** 

利用`[]`创建列表  

```python
# 创建一个空列表
empty_list = []

fruits = ["apple","banana","cherry"]
# 列表可以包含不同的数据类型
mixed_list = [1,"two",3.0,True,fruits]
```  

列表的访问主要通过索引和切片两种方式实现
1. 索引：访问单个元素
	- 正向索引：从列表的开头开始，0是第一个元素，1是第二个元素
	- 负向索引：从列表的末尾开始，-1是倒数第一个元素，-2是倒数第二个元素

如果访问索引超出列表范围，会抛出IndexError的错误

```python
fruits = ["apple","banana","cherry","orange","grape"]
# 正向索引
print(f"第一个元素: {fruits[0]}")  # 输出: apple
print(f"第三个元素: {fruits[2]}")  # 输出: cherry
# 负向索引
print(f"最后一个元素: {fruits[-1]}") # 输出: grape
print(f"倒数第二个元素: {fruits[-2]}") # 输出: orange
```

2. 切片：访问子列表`list[start:stop:step]`
	- `start`：起始索引（包含）。如果省略默认为开头（0）
	- `stop`：结束索引（不包含）。如果省略，默认为结尾
	- `step`：步长。如果省略，默认为1

```python
fruits = ["apple","banana","cherry","orange","grape"]
print(f"切片结果: {fruits[1:4]}")  
# 输出: ['banana','cherry','orange']
print(f"省略 start: {fruits[:3]}") 
# 输出: ['apple','banana','cherry']
print(f"省略 stop: {fruits[2:]}") 
# 输出: ['cherry', 'orange', 'grape']
print(f"带步长: {fruits[::2]}") 
# 输出: ['apple','cherry','grape']
print(f"列表反转: {fruits[::-1]}") 
# 输出: ['grape','orange','cherry','banana','apple']
```

值得注意的是，将一个列表赋值给另一个变量时，它们实际上指向了内存中的同一个列表对象，修改一个，另一个也会跟着改变  

```python
original_list = [1, 2, 3]
new_list = original_list  # 这不是拷贝，而是引用
new_list.append(4)
print(f"原始列表: {original_list}")
# 输出: 原始列表: [1, 2, 3, 4] （被修改了！）
```

解决方法是使用切片`[:]`或`copy()`方法创建浅拷贝，得到一个独立的新列表  

```python
original_list = [1, 2, 3]
new_list = original_list.copy() # 或original_list[:]
new_list.append(4)
print(f"原始列表: {original_list}")
# 输出: 原始列表: [1, 2, 3]
print(f"新列表: {new_list}")
# 输出: 新列表: [1, 2, 3, 4]
```

**常用方法：**

| 方法                        | 描述                              | 实例                                 |
| ------------------------- | ------------------------------- | ---------------------------------- |
| `list.append(item)`       | 在列表末尾添加元素                       | `fruits.append("kiwi")`            |
| `list.insert(index,item)` | 在指定索引处插入元素                      | `fruits.insert(1,"grape")`         |
| `list.extend(list)`       | 将一个可迭代对象中所有元素添加到当前列表末尾          | `fruits.extend(["mango","fig"])`   |
| `list.remove(item)`       | 删除列表中第一个匹配的元素                   | `fruits.remove("banana")`          |
| `list.pop(index)`         | 删除指定索引的元素并返回该元素，如果省略索引，默认删除最后一个 | `last_fruit = fruits.pop()`        |
| `list.sort()`             | 在原地对列表进行升序排列                    | `numbers.sort()`                   |
| `list.reverse()`          | 在原地反转列表元素顺序                     | `fruits.reverse()`                 |
| `list.index(item)`        | 返回第一个匹配元素的索引                    | `idx=fruits.index("cherry")`       |
| `list.count(item)`        | 返回元素在列表中出现的次数                   | `num_apples=fruits.count("apple")` |
| `list.copy()`             | 返回列表的一个浅拷贝                      | `new_list=my_list.copy()`          |

---

### dict

字典以键值对的形式储存数据
- 键的唯一性：一个字典不能有两个相同的键，如果用已有的键来赋值，旧的值会被新值覆盖
- 键的不可变性：字典的键必须是不可变的数据类型，如字符串、数字或元组。列表、集合和字典不能作为键
- 无序性：字典的插入一般是无序的，但从python3.7开始，字典是插入有序的。

**基本用法：**   

使用`{}`来创建字典，或者使用`dict()`函数   

```python
# 创建一个空字典
empty_dict = {}

person = {"name":"Bob","age":30,"city":"New York"}
```

通过键来访问、修改或添加元素  

```python
person = {"name":"kekdou","age":19}
# 访问元素
print(f"名字是：{person['name']}")   # 输出：kekdou
# 修改元素的值
person["age"] = 18
print(f"修改后的年龄：{person['age']}")   # 输出：18
# 添加新元素
person["city"] = "Peking"
print(f"添加新元素后：{person}")
# 输出：｛'name':'kekdou','age':18,'city':'Peking'｝
```

**常用方法：**   

| 方法                        | 描述                                           | 实例                                  |
| ------------------------- | -------------------------------------------- | ----------------------------------- |
| `dict.keys()`             | 返回一个由所有键组成的视图                                | `person.keys()`                     |
| `dict.values()`           | 返回一个由所有值组成的视图                                | `person.values()`                   |
| `dict.items()`            | 返回一个由所有键值对（元组）组成的视图                          | `person.items()`                    |
| `dict.get(key,default)`   | 获取指定键的值，若键不存在则返回default                      | `person.get('age',0)`               |
| `dict.update(other_dict)` | 使用另一个字典的键值对来更新当前字典，若键不存在会添加到字典中，若键已存在则会将新值覆盖 | `person.update({'job':'Engineer'})` |
| `dict.pop(key,default)`   | 移除指定键的键值对，并返回其值                              | `age=person.pop('age')`             |
| `dict.copy()`             | 返回字典的一个浅拷贝                                   | `new_dict=person.copy()`            |

---

### tuple

元组可以视作常列表，有以下特点：
- 不可变：元组被创建后元素不能被修改、添加或删除。这保证了数据的完整性
- 有序：和列表一样，元素有固定的顺序，可以通过索引来访问
- 异构：可以包含不同数据类型的元素
- 允许重复：元组中可以包含多个相同的元素

**基本用法：**   

创建元组用`()`   

```python
# 创建一个空元组
empty_tuple = ()

coordinates = (10,"hellp",3.14)
# 创建单元素元组时，必须在元素后加一个逗号
single_item_tuple = (5,)
```

*尤其要注意单元组的语法！*

元组的访问和列表完全相同：

```python
letters = ('a', 'b', 'c', 'd', 'e')
# 访问单个元素
print(f"第一个元素: {letters[0]}")  # 输出: a
# 负向索引
print(f"最后一个元素: {letters[-1]}") # 输出: e
# 切片
print(f"切片结果: {letters[1:4]}")  # 输出: ('b','c','d')
```

任何看似修改元组的操作，实际上都是创建了一个新的元组  

并且由于元组的不可变性，元组可以作为字典的键  

**常用方法：**   

| 方法                  | 描述            | 实例                    |
| ------------------- | ------------- | --------------------- |
| `tuple.count(item)` | 返回元素在元组中的出现次数 | `my-tuple.count(2)`   |
| `tuple.index(item)` | 返回元素第一次出现的索引  | `my_tuple.index('b')` |

---

### set

set是python中非常独特的数据结构，擅长处理唯一且无序的数据  
- 无序：集合中的元素没有固定的顺序，不能像列表那样通过索引来访问或修改元素
- 唯一性：集合会自动移除所有重复的元素，确保每个元素都是唯一的 
- 元素可哈希：集合中的元素必须是不可变的类型，例如数字，字符串或元组

**基本用法：**   

创建集合有两种方式  

```python
# 从一个可迭代对象（如列表）创建集合，会自动去重
my_list = [1,2,3,2,4,1]
my_set = set(my_list)
print(f"从列表创建的集合：{my_set}")  # 输出：{1,2,3,4}
# 使用花括号{}创建
my_set2 = {5,6,7}
print(f"使用花括号创建的集合：{my_set2}")  # 输出：{5,6,7}

# 注意：创建空集合必须使用set()，使用{}创建的是空字典
empty_set = set()
```

可以使用add( )或update( )方法来添加元素，使用remove( )或discard( )来删除元素 ，使用discard( )删除不存在的元素不会报错   

**集合运算**   

集合最强大的地方在于它的数学运算，可以高效的处理数据  

| 方法                  | 运算符    | 描述         | 实例                    |
| ------------------- | ------ | ---------- | --------------------- |
| `s.union(t)`        | `s\|t` | 返回两个集合的并集  | {1,2}\|{2,3}->{1,2,3} |
| `s.intersection(t)` | `s&t`  | 返回两个集合的交集  | {1,2}&{2,3}->{2}      |
| `s.difference(t)`   | `s-t`  | 返回两个集合的差集  | {1,2}-{2,3}->{1}      |
| `s.issubset(t)`     | `s<=t` | 检查s是否是t的子集 | {1,2}<={1,2,3}->TRUE  |
| `s.issuperset(t)`   | `s>=t` | 检查s是否是t的超集 | {1,2,3}>={1,2}->TRUE  |
