# Python 基础

在 Shell 中直接启动 `python` 应用程序，会进入 Python 的交互模式，即 REPL —— Read-Evaluation-Print Loop（读取、求值、打印循环）。从这个角度来说，Python 和 Shell 程序有很多相似之处。

用户通过键入 Python 语法的语句，将外部数据输入或载入进这个环境中，以及对这些数据进行操作。用户也可以将 Python 语句写入文件中（扩展名为 `.py`），即 Python 脚本，并使用 Python 程序执行这个脚本。

## 数据类型

### 值

**数**：形如 `123`、`45.6` 这样的值为数。

**字符串**：即一连串字符，以单引号或双引号包裹。如 `"apple"`、`'banana'`。多行字符串以三个连续的引号包裹：

```python
"""
越过长城，走向世界。
  Across the Great Wall we can reach every corner in the world.
"""
```

多行字符串，会以原状存储起来。上述字符串以字符串字面量表示即为 `'\n越过长城，走向世界。\n  Across the Great Wall we can reach every corner in the world.\n'`。

直接写出一个值，即定义了一个值。但是因为没有名称，我们不能使用，因此没有实际意义。于是，我们需要将名称绑定在值上，像下面这样。

```python
x = 1.234
y = "Hello, Beijing!"
```

我们也可以将新的名称绑定在已有的名称对应的值上：

```python
x = 123
y = x
```

数值类型一经产生，便不可修改。我们也将值称作“不可变类型”。

不同于“1.2”“3.45”等朴素值（primitive type），字符串有若干成员方法，我们可以调用这些方法得到**新的字符串对象**。

```python
s1 = 'hello'
s2 = s1.upper()    # s2: 'HELLO'
s3 = 'abc'.upper() # s3: 'ABC'
```


### 类

#### 容器

最常使用应属**容器**类了：Python 中提供存放多个元素的容器——列表（list），以及关联容器——字典（dict）等。

类似的，我们也可以将名字捆绑到类的实例——对象上：

```python
l = list()
d = dict()
```

对于列表和字典，Python 提供有简单的书写方式：

```python
l = []  # 空列表
d = {}  # 空字典
```

我们以方括号 `[]` 的形式来表示一个**列表**。列表中的元素不限种类。也可以将已有的名称放入列表中。我们可以将不具名的元素视作**匿名**值或匿名对象。

```python
x = 1
y = [2, 3]
z = "banana"

l = [x, y, 2.3, "apple", z, [4,5,6]]
```

我们可以使用**元组**将若干元素绑定在一起。需要注意，若干元素一经捆绑生成元组后，便不能再对该元组中元素进行增加或删除的操作。

```python
age = 21
things = ["phone", "keys"]
t = ("xiaoming", age, things)
```

下面的例子可以介绍名称即“引用”的概念，即，`x` 是对一个列表的引用，虽然将其与 `y` 一并建立生成了一个元组，但我们仍能通过这个引用修改对应的列表。

```python
x = [1,2,3]
y = 'apple'
t = (x, y)
t[0].append('4')
print(x) # 输出：[1, 2, 3, '4']
```

以其中含有键值对的花括号 `{}` 的形式来表示一个**字典**。字典要求键的类型为值。

```python
fruit_colors = {'apple':['red'], 'banana':['yellow'], 'pitaya':['green', 'red']}
students = [{'name':'Jason', 'age': 13}, {'name':'Henry', 'age': 14}]
```

**集合**（set）要求其中的元素不重复。

```python
colors = {'red','blue','yellow'}

l = ['apples', 'bananas', 'cherries', 'apples']
s = set(l) # s: {'apple', 'banana', 'cherries'}
```

列表中的元素从概念上是有序的，即，可以通过数字下标访问第 n 个元素。字典中的键值对、集合中的元素，在概念上应当理解为无序的，即，两个对象之间不存在逻辑上的先后顺序。

#### 索引容器中的元素

对于容器，我们通过方括号运算符 `[]` 进行索引，取得对其中的元素的**引用**。对于列表和元组，使用数字（下标）访问。对于字典，我们用“键”来访问。

使用下表访问列表中的元素：

```python
menu = ['dishes', 'drinks', 'fruit']
print(menu[0])    # 输出：dishes
```

使用下表访问元组中的元素：

```python
person = ("xiaoming", 21, ["phone", "keys"])
print(person[1])  # 输出：21
```

使用键访问关联容器（字典）中对应的值：

```python
color = {'apple':['red'], 'banana':['yellow'], 'pitaya':['green', 'red']}
print(color['apple'])          # 输出：['red']

color['apple'].append('green')
print(color['apple'])          # 输出：['red', 'green']
```

```python
students = [{'name':'Jason', 'age': 13}, {'name':'Henry', 'age': 14}]
stu = students[0]
print(stu['name'])   # 输出：Jason
print(stu[1]['age']) # 输出：14
```

### 可调用对象

也有函数类型，我们也可以将其视作**可调用**对象。

定义一个函数。函数的参数列表为函数中要使用的名称。我们通过括号运算符 `()` 来对一个对象执行调用操作。

```python
def get_hello(): 
    return 'hello'


h = get_hello()  # h: 'hello'
```

```python
def push_back_letter_s(some_list):
    some_list.append('s')


l = ['a', 'b']
push_back_s(l)   # l: ['a', 'b', 's']
```

同样，我们也可以将一个名称绑定到函数对象上。

```python
def to_plural_append_s(s):
    return s + 's'

f = to_plural_append_s
s1 = 'apple'
s2 = f(s)        # s2: 'apples'
```

对于一些简单的操作，我们可以使用 lambda 表达式。

```python
append_s = lambda x : x + 's'

words = ['apple', 'banana', 'melon']

plural = list(map(append_s, words))

# plural: ['apples', 'bananas', 'melons']
```

> `map(f, l)` 会生成一个 `map` 对象，其将 `f` 操作应用在 `l` 中的每一个元素上。该 `map` 对象不能直接访问其中元素，但可以被迭代，比如使用 `for` 循环，同理，亦可使用其构造一个列表，即 `list(map(f,l))`。

## 字符串、容器之间的转换

字符串是可迭代对象。因此，我们可以从字符串生成一个列表。

```python
s = 'hello'
l = list(s)
# l: ['h', 'e', 'l', 'l', 'o']
```

同理，也能生成一个集合，集合内就是字符串中出现的所有的字符。

```python
s = 'hello'
p = set(s)
# p: {'l', 'h', 'e', 'o'}
```

字符串是不能修改的（任何对字符串的操作会产生一个新的对象），但可以修改列表中某个位置对于对象的引用。

```python
s = 'hello'
l = list(s)
# l: ['h', 'e', 'l', 'l', 'o']
l[1] = 'a'
# l: ['h', 'a', 'l', 'l', 'o']
```

使用某字符串的 `join` 方法可以将列表中的字符串拼接在一起，并以该字符串为间隔。

```python
l = ['h', 'e', 'l', 'l', 'o']
s1 = "".join(l) # s1: 'hello'
s2 = "-".join(l) # s2: 'h-e-l-l-o'
```
