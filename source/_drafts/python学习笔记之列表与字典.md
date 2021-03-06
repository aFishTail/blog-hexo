---
title: python学习笔记之列表与字典
categories: 学习笔记
tags: Python
---
## 列表

## 字典
### 概述
除了列表以外，字典（dictionary）也许是Python之中最灵活的内置数据结构类型。如果把列表看做是有序的对象集合，那么就可以把字典当成是 **无序** 的集合。它们主要的差别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。
与列表不同，保存在字典中的项并没有特定的顺序。实际上，Python将各项**从左到右随机排序**，以便快速查找。键提供了字典中项的象征性（而非物理性的）位置。
### 实际应用
#### 基本操作
- 使用键值访问，修改字典
```
 >>> D = {'span':2,'ham':1,'eggs':3}
 >>> D['span']
```
- 内置len函数也可以用于字典，能够返回存储在字典里元素的数目，或说是其keys()列表的长度，这二者是等价的。典的has_key方法以及in成员关系操作符提供了键存在与否的测试方法，keys方法则能够返回字典中所有的键，将它们收集在一个列表中。后者对于按顺序处理字典是非常有用的，但是你不应该依赖keys列表的次序。然而，因为keys结果是一个普通列表，如果次序要紧，随时都可以进行排序：
```
>>> len(D)
3
>>> 'ham' in D
True
>>> list(D.keys())
['span', 'ham', 'eggs']
>>>
```
**注意**  
Python 3.0中的keys返回一个迭代器，而不是一个物理的列表。list调用迫使它一次生成所有的值，以便我们可以将其打印出来。
#### 原处修改字典
与列表相同，字典也是可变的，因此可以在原处对它们进行修改、扩展以及缩短而**不需要生成新字典**。  
Python中，所有集合数据类型都可以彼此任意嵌套。
```
>>> list(D.keys())
['span', 'ham', 'eggs']
>>> D['ham']=['grill','bake','fry'] 
>>> D
{'span': 2, 'ham': ['grill', 'bake', 'fry'], 'eggs': 3}
>>> del D['eggs']
>>> D
{'span': 2, 'ham': ['grill', 'bake', 'fry']}
>>> D['brunch'] = 'Bacon'
>>> D
{'span': 2, 'ham': ['grill', 'bake', 'fry'], 'brunch': 'Bacon'}
```
与列表相同，向字典中已存在的索引赋值会改变与索引相关联的值。然而，与列表不同的是，每当对新字典键进行赋值（之前没有被赋值的键），就会在字典内生成一个新的元素，就像前一个例子里对'brunch'所做的那样。在列表中情况不同，因为Python会将超出列表末尾的偏移视为越界并报错。要想扩充列表，你需要使用append方法或分片赋值来实现。
#### 其他字典方法
- 字典value s和items方法分别返回字典的值列表和(key,value)对元组
```
>>> D = {'spam':2,'ham':1,'eggs':3}
>>> list(D.values())
[2, 1, 3]
>>> list(D.items())  
[('spam', 2), ('ham', 1), ('eggs', 3)]
>>>
```
读取不存在的键往往会报错，使用**get**方法可以获取默认值（None或者用户定义的默认值）
```
>>> D['toast']
Traceback (most recent call last):   
  File "<stdin>", line 1, in <module>
KeyError: 'toast'
>>> D.get('toast')
>>> D.get('toast', 88)
88
```
字典的**update**方法有点类似于合并，但是，它和从左到右的顺序无关（再一次强调，字典中没有这样的事情）。它把一个字典的键和值合并到另一个字典中，盲目地覆盖相同键的值：
```
>>> D2 = {'toast':4,'muffs':5} 
>>> D.update(D2)
>>> D
{'spam': 2, 'ham': 1, 'eggs': 3, 'toast': 4, 'muffs': 5}
```
最后，字典**pop**方法能够从字典中删除一个键并返回它的值。这类似于列表的pop方法，只不过删除的是一个键而不是一个可选的位置：
```
>>> D
{'spam': 2, 'ham': 1, 'eggs': 3, 'toast': 4, 'muffs': 5}
>>> D.pop('muffs') // 根据key删除并返回一个值
5
>>> D.pop('toast') 
4
>>> D
{'spam': 2, 'ham': 1, 'eggs': 3}
>>> L = ['aa','bb','cc','dd']
>>> L.pop() // 从尾部删除
'dd'
>>> L
['aa', 'bb', 'cc']
>>> L.pop(1) // 根据索引删除
'bb'
>>> L
['aa', 'cc']
>>>
```
#### 创建字典的其他方法
##### 字典解析
  **zip**函数是在一个单个调用中从键和值的列表来构建一个字典的方式之一
  ```
  >>> list(zip(['a','b','c'],['1','2','3']))
  [('a', '1'), ('b', '2'), ('c', '3')]
  >>> D = dict((zip(['a','b','c'],['1','2','3'])))
  >>> D
  {'a': '1', 'b': '2', 'c': '3'}
  ```  
  也可以使字典解析表达式达到同样的效果  
  要真正理解字典解析，我们还需要了解Python中有关迭代语句和概念的更多知识



