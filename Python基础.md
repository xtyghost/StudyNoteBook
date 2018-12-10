### 直接执行代码

```python
#!/usr/bin/env python3
```

### 注释

```python
# 单行注释

'''
多行注释
多行注释
'''
```

### 设置编码

```python
#coding=utf-8
# -*- coding: utf-8 -*-
```

### 变量

```python
# 定义变量
变量名 = 值
# 查看变量类型
type(变量名)
```

### 字符串

```python
# 获取单个字符编码
ord(字符)
# 字符编码转换为字符
chr(编码)

# 字符串转为bytes
字符串.encode(编码)
# bytes转为字符串
bytes.decode(编码)

# 获取字符串长度
len(字符串)
# 字符串转换成int
int(字符串)

# 原始字符串
r字符串
```

### 运算符

```python
** 幂
// 取整除
```

### print

```python
# 格式化
print('%s' %字符串)
print('%s %d' %(字符串,整数))
%d 整数
%f 浮点数
%s 字符串
%x 16进制数
```

### 语法

```python
# 条件判断
if 条件:
    执行语句
elif 条件:
    执行语句
else:
    执行语句
   
# for循环
for 变量 in 集合:
    执行语句
    
# while循环
while 条件:
    执行语句
    
# 占位符
pass
```

#### list

```python
# 定义list
列表名 = [元素1, 元素2, ...]
# list长度
len(列表名)
# 取元素
列表名[0] # 取第一个元素
列表名[-1] # 取最后一个元素
# 尾部追加元素
列表名.append(元素)
# 指定位置插入元素
列表名.insert(位置, 元素)
# 删除尾部元素
列表名.pop()
# 删除指定位置元素
列表名.pop(位置)

# 迭代
for 值 in list
for 索引, 值 in enumerate(list)
```

#### tuple(元组)

tuple与list类似,但不可修改

```python
# 定义tuple
元组名 = (元素1, 元素2, ...)
```

#### dict(字典)

```python
# 定义dict
字典名 = {键1:值1,键2:值2...}
# 根据键取值
字典名[键]
字典名.get(键)
# 判断一个键是否在字典里
键 in 字典
# 删除键值
字典名.pop(键)

# 迭代
for 键 in dict
for 键 in dict.values()
for 键, 值 in dict.items()
```

#### set

```python
# 定义set
set名 = set(list)
# 添加元素
set名.add(值)
# 删除元素
set名.remove(值)
# set交集
set1 & set2
# set并集
set1 | set2
# set差集
set1 - set2
```

### 函数

```python
# 定义函数
def 函数名(参数1, 参数2...):
    函数体
# 参数默认值
def 函数名(参数1, 参数2=默认值):
    函数体
# 可变参数 自动组装为tuple
def 函数名(*参数名):
    函数体
# 关键参数 自动组装为dict
def 函数名(**参数名):
    函数体
# 命名关键字参数
def 函数名(*, 参数1, 参数2...):
    函数体
```

### 高级特性

#### 切片

```python
# 取元素
对象[开始:结束]
对象[开始:结束:间隔]
```

#### 迭代

```python
for key in 对象
```

## #

+ 包 (文件夹)
  + 模块 (文件)
    + 类

#### 包

文件夹包含`__init__.py`文件

### 定义类

```python
class 类名(父类名):
    字段
    __私有字段
    # 构造函数
    def __init__(self):
       函数体
    # 普通函数
    def 函数名(self):
        函数体
    # 类函数
    @classmethod
    def 函数名(cls):
        函数体
    # 静态函数
    @staticmethod
    def 函数名():
        函数体
    # 私有函数
    def __函数名():
        函数体 
        
    # 调用父类方法
    super(本类名,self).父类方法
```

### 正则表达式

```python
import re
re.findall('正则表达式', 字符串)
```

### json

```python
import json
字典 = json.loads(json字符串)
json字符串 = json.dumps(字典)
```

### 枚举

```python
from enum import Enum
# 定义枚举
class 枚举名(Enum):
    name = value
    
枚举名(value)
```

