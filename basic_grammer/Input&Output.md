# 输入输出

---

## 格式化输入输出

Python中的格式化输入输出主要有：`f-string`、`str.format()`、和`%`三中方式    

---

### f-string

f-string是Python3.6引入的新方式，也是目前最常用的方式，具有简洁，可读性高，执行速度快的特点  

- 基本使用：

```python
name = "张三"
age = 30 
print(f"你好，我的名字是{name}，我今年{age}岁。”)
# 输出：你好，我的名字是张三，我今年30岁
```

- 也可以在{ }内进行表达式计算：

```python
price = 19.9
quantity = 5
print(f"总价是：{price * quantity:.2f}元")  # .2f表示保留两位小数
# 输出：总价是99.95元
```

- 格式化对齐：
	- `:<`左对齐
	- `:^`居中对齐
	- `:>`右对齐
	- `width`指定宽度(跟在后面的数字)

```python
print(f"|{'姓名':<10}|{'年龄':>5}|")
print(f"|{'李四':<10}|{25:>5}|")
# 输出:
# |姓名      |   年龄|
# |李四      |   25|
```

*注意此处姓名等要加上`''`，否则会被编译为变量名导致错误*

---

### str.format()

str.format()方法是Python2.6引入的，功能与f-string类似，但语法更加复杂  
这种方法兼容性更强，适用于python2.6及更高版本，且更加灵活  

- 按顺序传参：

```python
name = "李四"
age = 25
print("你好，我的名字是{}，我今年{}岁。".format(name, age))
# 输出: 你好，我的名字是李四，我今年25岁。
```

- 按索引传参：

```python
print("我喜欢{0}和{1}，但更喜欢{0}。".format("苹果", "香蕉"))
# 输出: 我喜欢苹果和香蕉，但更喜欢苹果。
```

- 按关键字传参：

```python
print("我的名字是{name}，年龄是{age}。".format(age=30,name=kekdou))
# 输出：我的名字是kekdou，年龄是30
```

---

### % 运算符（printf风格）

最传统的格式化方式，类似C语言的printf，在python早期版本流行，现被逐渐取代   

先使用以下占位符，然后在字符串后面使用% 运算符传入变量
- `%s`：字符串
- `%d`：整数
- `%f`：浮点数

```python
name = "kekdou"
age = 22
gpa = 3.8
print("我的名字是%s，我今年%d岁，绩点%.2f。"%(name,age,gpa))
# 输出：我的名字是kekdou，我今年22岁，绩点3.8
```

---

## 标准输入输出

标准输入输出主要通过`print()`和`input()`两个内置函数实现  

---

### print( )

print( )会在每次调用后自动换行，可以传入一个或多个值，用空格隔开  

- 基本用法：

```python
print("Hello,world!")
# 输出：Hello,world!

age = 30
print("我的年龄是",age,“岁”)
# 输出：我的年龄是30岁
```

- 常用参数：
1. `sep`(separator)：用于指定多个值之间分隔符，默认是一个空格

```python
print("apple","banana","orange")
# 输出：apple banana orange

print("apple","banana","orange",sep="|")
# 输出：apple|banana|orange

print("2025","09","15",sep="-")
# 输出：2025-09-15
```

2. `end`(ending)：用于指定输出的结尾符，默认是换行符 `\n`，如果想让输出在同一行，可以使用 `end=""`或其他字符  

```python
print("第一部分",end=" ")
print("第二部分")
# 输出：第一部分 第二部分

for i in range(3):
	print(i,end="...")
# 输出：0...1...2...
```

---

### input( )

input( )函数用于获取用户在命令行输入的数据，会暂停程序直到用户输入内容并按下回车  

- 基本用法：
input( )接收一个可选的字符串参数，作为提示信息显示给用户，并且**input( )返回的所有数据类型都是字符串（str）**  

```python
name = input("请输入你的名字：")
print("你好，" + name)    # str可以用 + 连接
```

- 类型转换：   
由于input( )返回的是字符串，如需处理数字，必须使用`int()`或`float()`函数进行类型转换  

```python
age_str = input("请输入你的年龄：")
# 'age_str'是一个字符串

# 将字符串转换为整数
age = int(age_str)
# 现在可以进行数学运算
print("你明年就",age+1,"岁了。")
```

下面给出一个综合运用：

```python
# 1.使用input()获取用户的名字
name = input("请问你的名字是？")

# 2.使用input()获取用户的年龄，并立即转换为整数
# 加上错误处理
try:
    age = int(input(f"好的，{name}，你今年多大了？"))
    
    # 3.使用print()和f-string格式化输出
    print(f"{name}，你好！明年你就 {age + 1} 岁了。")
    
except ValueError:
    print("输入的年龄不是一个有效的数字。")
```

---

## 文件流

主要依靠open( )创建文件对象，然后使用对象的各种方法来实现读取和写入  

---

### open（ ）

所有文件操作的第一布都是使用内置的open( )函数来创建一个文件对象  

基本语法：`open(file,mode='r',encoding=None)`     
- `file`：必需参数，文件的路径和名称
- `mode`：可选参数，指定打开文件的目的
- `encoding`：可选参数，指定文件编码格式，通常使用`utf-8`来处理中文等多种语言字符  

常见的文件打开模式（mode）：

| 模式    | 描述                                       | 文件不存在时 | 文件存在时       |
| ----- | ---------------------------------------- | ------ | ----------- |
| `'r'` | 读取（read），默认模式                            | 报错     | 正常打开        |
| `'w'` | 写入（write）                                | 创建新文件  | 清空文件内容，然后打开 |
| `'a'` | 追加（append）                               | 创建新文件  | 在文件末尾追加新内容  |
| `'t'` | 文本模式（text），默认模式                          | N/A    | N/A         |
| `'b'` | 二进制模式（binary），用于处理图片，音频等非文本文件            | N/A    | N/A         |
| `+`   | 更新（update），与`'r'`、`'w'`、`'a'`结合使用，表示可读可写 | N/A    | N/A         |

以`r+`为例：
1. 文件必须存在，否则抛出FileNotFoundError错误
2. 文件打开后指针默认位于开头
3. 读写行为
	- 读取：可以从文件开头开始读取内容
	- 写入：从当前光标开始覆盖原有内容

文件操作中，非常重要的概念是资源管理，打开一格文件后，程序需要显式的关闭它来释放系统资源，如果忘记关闭或者程序在操作过程中报错，会导致资源泄露  

`with`语句是解决这个问题的最佳方案，能确保文件在使用完毕后，无论是否发生异常，都会被**自动关闭**     

相当于在with的作用域内进行安全操作

```python
# 推荐写法：使用with语句
with open('example.txt','r') as file:
	content = file.read()
	print(content)
	
# 避免这种写法：需要手动关闭
file = open('example.txt','r')
content = file.read()
print(content)
file.close()   # 容易忘记或因错误而跳过
```

---

### 读写方法

一旦使用open( )创建了文件对象，就可以使用其方法来读写数据  

 **读取文件**    
 - `file.read(size)`：读取文件的所有内容或指定字节数，并作为一个**字符串**返回。这对于小文件很方便
 - `file.readline()`：读取一行内容，返回一个**字符串**，每次调用会从当前位置读取一行，包括换行符
 - `file.readlines()`：读取所有行，返回一个包含所有行的**列表**，但会一次性加载所有内容，容易导致内存溢出
 - `for line in file:`：最推荐的迭代方式，懒加载，每次只加载一行到内存，效率高且节省内存

```python
with open('data.txt','r',encoding='utf-8') as file
	for line in file:
		processed_line = line.strip()   # 使用strip（ ）方法来移除行末的换行符和空格
		print(processed_line)
```

**写入文件**    
- `file.write(string)`：将指定的字符串写入文件，但不会自动添加换行符，需手动添加`\n`
- `file.writelines(list)`：将一格字符串列表写入文件，同样不会自动添加换行符   

```python
# 写入字符串
with open('output.txt','w',encoding='utf-8') as file:
	file.write('这是第一行。\n')
	file.write('这是第二行。')
	
# 写入字符串列表
lines = ["第一行","第二行","第三行"]
with open("output.txt",'w'''encoding='utf-8') as file
	# 确保每一行都以换行符结尾
	file.writelines(line + '\n' for line in lines)	
```

综合实例，先写入数据，然后读出来：

```python
# 写入会清空旧内容
with open('my_data.txt','w',encoding='utf-8') as file:
	file.write("Python文件输入输出\n")
	file.write("这是一种流的概念。\n")
	file.write("使用with语句是最佳实践。\n")
	
# 追加能在文件末尾添加新内容
with open('my_data.txt','a',encoding='utf-8') as file:
	file.write("我们可以继续追加内容。\n")
	
# 读取
with open('my_data.txt','r',encoding='utf-8') as file:
	print("---文件内容---")
	for line in file:
		print(line.strip())
```

**读写模式**    

对于 `r+`，`w+` ，`a+`等模式，只需注意写入为覆写  

如果有一个文件 `greeting.txt`，其内容如下：

```python
Hello World!
```

利用 `r+`模式打开，并进行读写操作：

```python
with open('greeting.txt','r+') as file:
	file.write("Hi")
	file.seek(0)
	content = file.read()
	print(content)
```

此时输出为：

```python
Hillo World!
```

--- 

### 光标操作

Python提供tell( )和seek( )两个方法来精确调控光标  

**`file.tell()`**：查看光标位置
- 返回一个整数，表示从文件开头算起的字节数
- 用于追踪在文件的位置，或者保存某个位置

```python
with open('example.txt,'w') as file:
	file.write("Hello")
	file.write("World")
	print(f"当前光标位置：{f.tell()}") # 输出：当前光标位置：10
```

**`file.seek(offset,whence=0)`**：移动光标位置    
- `offset`：一个整数，代表要移动的字节数
- `whence`：一个整数，代表移动的参照点。最重要的参数

**whence的三个值**   

| whence值 | 符号常量        | 参照点  | 描述                         |
| ------- | ----------- | ---- | -------------------------- |
| 0       | os.SEEK_SET | 文件开头 | 默认值，offset必须是正数或0          |
| 1       | os.SEEK_CUR | 当前位置 | offset可以是正数（向后），也可以是负数（向前） |
| 2       | os.SEEK_END | 文件结尾 | offset通常为负数或0，从末尾向前移动      |

值得注意的是，在文本模式下，seek( )的offset参数只能是0或者由tell( )返回的值。这是因为不同编码中的字符所占字节数不同，python无法准确计算非零offset对应的字节位置

如果要精确的字节级别控制，须使用二进制模式（`'b'`）   

```python
with open('example.txt','rb') as file:
	file.read(5)   # 读取“Hello”，光标移动到第5个字节
	file.seek(3,1)    # 从当前位置向后移动3个字节
	content = file.read()
	print(content.decode)   # 输出：rld
```

decode( )方法的作用是将字节数据转换为可读的字符串，在二进制模式相下，read( )等方法返回的是字节对象，而非字符串，这时想看文件内容则需要使用decode( )   

```python
with open('greeting.txt','rb') as file:
	byte_data = file.read()
	# 此时，byte_data 的值是：b'\xe4\xbd\xa0\xe5\xa5\xbd'
	print(f"原始字节数据：{byte_data}")
	# 使用decode()解码
	string_data = byte_data.decode('utf-8')
	# 此时，string_data 的值是："你好"
	print(f"解码后的字符串：{string_data}")
```

encode( )作用与decode( )相反，将字符串转换为字节，成为编码  

