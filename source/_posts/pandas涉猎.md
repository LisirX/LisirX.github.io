---
title: pandas涉猎
mathjax: false
tags: ['Python', 'pandas', '信息技术', '']
categories: ['Python']
copyright: true
date: 2022-11-27 13:25:40
---

# pandas涉猎

高中信息技术必修一《数据与计算》中有关于`pandas`的一些知识，但总觉得无法贴切地理解，故稍作整理。参考网站：https://www.runoob.com/，https://geek-docs.com/。

<!-- More -->

## Series

`Series`是一种一维的数据结构，包含一个数组的数据和一个与数组关联的索引（`index`)，索引值默认是从0起递增的整数。列表、字典等都可以用来创建`Series`数据结构，与列表不同的是，`Series`的索引可以指定，类型可以为字符串型。

`Series`的结构如下：

```python
pandas.Series(data, index, dtype, name, copy)
```

参数说明：

|  参数   |                   说明                    |
| :-----: | :---------------------------------------: |
| `data`  |         一组数据(`ndarray`类型)。         |
| `index` | 数据索引标签，如果不指定，默认从 0 开始。 |
| `dtype` |        数据类型，默认会自己判断。         |
| `name`  |                设置名称。                 |
| `copy`  |         拷贝数据，默认为`False`。         |

### 实例1 - 从数组创建

通过数组来创建`Series`

代码：

```python
import pandas as pd
a = [1, 2, 3]
myvar = pd.Series(a)
print(myvar)
```

输出：

```shell
0    1
1    2
2    3
dtype: int64
```

### 实例2 - 访问元素

代码：

```python
import pandas as pd
a = [1, 2, 3]
myvar = pd.Series(a)
print(myvar[1])
```

输出：

```shell
2
```

这说明在没有指定`index`的情况下，`Series`的默认下标为`0,1,2,...`

### 实例3 - 改变索引

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki"]
myvar = pd.Series(a, index = ["x", "y", "z"])
print(myvar)
```

输出：

```shell
x    Google
y    Runoob
z      Wiki
dtype: object
```

可见我们可以指定`index`来改变默认索引

### 实例4 - 访问元素

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki"]
myvar = pd.Series(a, index = ["x", "y", "z"])
print(myvar["y"])
print(myvar[1])
print(myvar.y)
```

输出：

```shell
Runoob
Runoob
Runoob
```

可见我们改变索引后，可以通过我们设定的索引来访问元素（和字典有异曲同工之妙），也可以用默认索引来访问元素，还可以直接用访问元素的方式（这个方法只适用于索引为字符串的时候）来访问。

### 实例5 - 索引值相同的访问

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki", "Xiaoshi"]
myvar = pd.Series(a, index = ["x", "y", "z", "x"])
print(myvar)
print(myvar["x"])
```

输出：

```shell
x     Google
y     Runoob
z       Wiki
x    Xiaoshi
dtype: object
x     Google
x    Xiaoshi
dtype: object
```

可见`Series`可以指定同样的索引值，并且在访问时如果一个索引对应着不同的元素，会返回所有查询到的元素组成的`Series`

### 实例6 - 指定数字为索引

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki", "Xiaoshi"]
myvar = pd.Series(a, index = [1, 2, 3, 4])
print(myvar)
print(myvar[1])
```

输出：

```shell
1     Google
2     Runoob
3       Wiki
4    Xiaoshi
dtype: object
Google
```

可见如果指定`index`为数字，会覆盖掉默认的从0开始的索引

### 实例7 - 索引值相同的访问

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki", "Xiaoshi"]
myvar = pd.Series(a, index = [1, 2, 3, 1])
print(myvar)
print(myvar[1])
```

输出：

```shell
1     Google
2     Runoob
3       Wiki
1    Xiaoshi
dtype: object
1     Google
1    Xiaoshi
dtype: object
```

和实例5类似。

### 实例8 - 从字典创建

代码：

```python
import pandas as pd
sites = {1: "Google", 2: "Runoob", 3: "Wiki"}
myvar = pd.Series(sites)
print(myvar)
myvar = pd.Series(sites, index = [1, 2])
print(myvar)
myvar = pd.Series(sites, index = [1, 2, "x"])
print(myvar)
```

输出：

```shell
1    Google
2    Runoob
3      Wiki
dtype: object
1    Google
2    Runoob
dtype: object
1    Google
2    Runoob
x       NaN
dtype: object
```

可见我们也可以用字典来创建`Series`，并且可以指定索引。对于不存在的索引，会自动补上`NaN`

### 实例9 - 从数组创建不会出现`NaN`元素

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki", "Xiaoshi"]
myvar = pd.Series(a, index = [1, 2, 3, 4, 5])
print(myvar)
```

输出：

```shell
Traceback (most recent call last):
  File "D:/Download/2.py", line 3, in <module>
    myvar = pd.Series(a, index = [1, 2, 3, 4, 5])
  File "D:\Program Files\Python\Python38\lib\site-packages\pandas\core\series.py", line 461, in __init__
    com.require_length_match(data, index)
  File "D:\Program Files\Python\Python38\lib\site-packages\pandas\core\common.py", line 561, in require_length_match
    raise ValueError(
ValueError: Length of values (4) does not match length of index (5)
```

说明元素不存在（也即元素为`NaN`）的索引只可能在用字典创建时出现

### 实例10 - 切片操作

代码：

```python
import pandas as pd
a = ["Google", "Runoob", "Wiki", "Xiaoshi"]
myvar = pd.Series(a, index = ["a", "b", "c", "d"])
print(myvar.index)
print()
print(myvar)
print()
print(myvar.values)
print()
print(myvar[2])
print()
print(myvar[1:3])
print()
print(myvar[-2:])
print()
print(myvar[1:4:2])
print()
print(myvar[["b","c","d"]])
```

输出：

```shell
Index(['a', 'b', 'c', 'd'], dtype='object')

a     Google
b     Runoob
c       Wiki
d    Xiaoshi
dtype: object

['Google' 'Runoob' 'Wiki' 'Xiaoshi']

Wiki

b    Runoob
c      Wiki
dtype: object

c       Wiki
d    Xiaoshi
dtype: object

b     Runoob
d    Xiaoshi
dtype: object

b     Runoob
c       Wiki
d    Xiaoshi
dtype: object
```

提供了几种访问方法

### 其他的操作

#### `Series.add()`

```python
import pandas as pd
s = pd.Series([1,2,3])
print(s)
s = s.add(10)
print(s)
s = pd.Series(["a","b","c"])
print(s)
s = s.add("g")
print(s)
```

```shell
0    1
1    2
2    3
dtype: int64
0    11
1    12
2    13
dtype: int64
0    a
1    b
2    c
dtype: object
0    ag
1    bg
2    cg
dtype: object
```

#### `index`和`value`

```python
import pandas as pd
s = pd.Series(["a","b","c"])
for i in s:
    print(i)
print()
for i in s.values:
    print(i)
print()
for i in s.index:
    print(i)
print()
for i in s.index:
    print(s[i])
```

```shell
a
b
c

a
b
c

0
1
2

a
b
c
```



## DataFrame

`DataFrame`是二维数据结构，它包含一组有序的列，每列可以是不同的数据类型，`DataFrame`既有行索引，也有列索引，它可以看作是`Series`组成的集合，不过这些`Series`共用一个索引。

功能特点：

- 不同的列可以是不同的数据类型
- 大小可变
- 含行索引和列索引
- 可以对行和列执行算术运算

`DataFrame`的结构如下：

```python
pandas.DataFrame(data, index, columns, dtype, copy)
```

构造函数的参数说明如下：

|   参数    |                             说明                             |
| :-------: | :----------------------------------------------------------: |
|  `data`   | 支持多种数据类型，如:`ndarray`，`series`，`map`，`lists`，`dict`，`constant`和另一个`DataFrame`。 |
|  `index`  |     行标签，如果没有传递索引值，默认值为`np.arrange(n)`      |
| `columns` |     列标签，如果没有传递索引值，默认值为`np.arrange(n)`      |
|  `dtype`  |                       每列的数据类型。                       |
|  `copy`   |                是否复制数据，默认值为`False`                 |

下面介绍如何创建数据表格(**DataFrame**)。

### 创建一个空的 DataFrame

```python
#import the pandas library and aliasing as pd
import pandas as pd
df = pd.DataFrame()
print (df)
```

执行结果如下:

```shell
Empty DataFrame
Columns: []
Index: []
```

### 从列表创建 DataFrame

可以使用单个列表或二维列表创建数据表格(**DataFrame**)。

**例1**：单个列表创建DataFrame

```python
import pandas as pd
data = [1,2,3,4,5]
df = pd.DataFrame(data)
print (df)
```

执行结果如下:

```shell
   0
0  1
1  2
2  3
3  4
4  5
```

**例2**：二维列表创建DataFrame

```python
import pandas as pd
data = [['Alex',10],['Bob',12],['Clarke',13]]
df = pd.DataFrame(data,columns=['Name','Age'])
print (df)
```

执行结果如下:

```shell
     Name  Age
0    Alex   10
1     Bob   12
2  Clarke   13
```

**例3**：二维列表创建DataFrame，并指定dtype

```python
import pandas as pd
data = [['Alex',10],['Bob',12],['Clarke',13]]
df = pd.DataFrame(data,columns=['Name','Age'],dtype=float)
print (df)
```

执行结果如下:

```shell
     Name   Age
0    Alex  10.0
1     Bob  12.0
2  Clarke  13.0
```

> 注: 可以观察到，`dtype`参数将`Age`列的类型更改为浮点。

### 从 ndarrays/Lists 的字典来创建 DataFrame

所有的`ndarrays`必须具有相同的长度。如果传递了索引(`index`)，则索引的长度应等于数组的长度。
如果没有传递索引，则默认情况下，索引为`range(n)`，其中`n`为数组长度。

```python
import pandas as pd
data = {'Name':['Tom', 'Jack', 'Steve', 'Ricky'],'Age':[28,34,29,42]}
df = pd.DataFrame(data)
print (df)
```

执行结果如下:

```shell
   Age   Name
0   28    Tom
1   34   Jack
2   29  Steve
3   42  Ricky
```

> 注：观察值`0`,`1`,`2`,`3`，它们是分配给每个使用函数`range(n)`的默认索引。

使用列表作为索引，创建一个数据表格(**DataFrame**)。

```python
import pandas as pd
data = {'Name':['Tom', 'Jack', 'Steve', 'Ricky'],'Age':[28,34,29,42]}
df = pd.DataFrame(data, index=['rank1','rank2','rank3','rank4'])
print (df)
```

执行结果如下:

```shell
       Age   Name
rank1   28    Tom
rank2   34   Jack
rank3   29  Steve
rank4   42  Ricky
```

> 注：`index`参数为每行分配一个索引。

### 从字典列表创建 DataFrame

`字典列表`可作为输入数据用来创建数据表格(**DataFrame**)，字典键默认为**列名**。

**例1**：传递字典列表来创建数据表格(**DataFrame**)。

```python
import pandas as pd
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data)
print (df)
```

执行结果如下:

```shell
   a   b     c
0  1   2   NaN
1  5  10  20.0
```

> 注：观察到，使用`NaN`填写空白区域

**例2**：传递字典列表和行索引来创建数据表格(**DataFrame**)。

```python
import pandas as pd
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data, index=['first', 'second'])
print (df)
```

执行结果如下:

```shell
        a   b     c
first   1   2   NaN
second  5  10  20.0
```

**例3**：以下示例显示如何使用字典，行索引和列索引列表创建数据表格(**DataFrame**)。

```python
import pandas as pd
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]

#With two column indices, values same as dictionary keys
df1 = pd.DataFrame(data, index=['first', 'second'], columns=['a', 'b'])

#With two column indices with one index with other name
df2 = pd.DataFrame(data, index=['first', 'second'], columns=['a', 'b1'])
print (df1)
print (df2)
```

执行结果如下:

```shell
        a   b
first   1   2
second  5  10
        a  b1
first   1 NaN
second  5 NaN
```

> 注：`df1`是使用列索引创建的，与字典键相同
> `df2`使用字典键以外的列索引创建`DataFrame`，使用`NaN`填写空白区域

### 从 Series 字典来创建 DataFrame

通过传递 Series 字典来创建DataFrame，最终索引是两个Series索引的并集。

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
      'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print (df)
```

执行结果如下:

```shell
   one  two
a  1.0    1
b  2.0    2
c  3.0    3
d  NaN    4
```

> 注：对于第一个`Series`，观察到没有包含索引`'d'`，输出结果中，对应索引`d`区域，填写NaN。

### DataFrame 读取列

下面将从数据表格(DataFrame)中读取一列。

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
      'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print (df['one'])
```

执行结果如下:

```shell
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64
```

### DataFrame 添加列

下面演示如何向一个 DataFrame 中添加一个新列

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
      'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)

# Adding a new column to an existing DataFrame object with column label by passing new series

print ("Adding a new column by passing as Series:")
df['three']=pd.Series([10,20,30],index=['a','b','c'])
print (df)

print ("Adding a new column using the existing columns in DataFrame:")
df['four']=df['one']+df['three']

print (df)
```

执行结果如下:

```shell
Adding a new column by passing as Series:
   one  two  three
a  1.0    1   10.0
b  2.0    2   20.0
c  3.0    3   30.0
d  NaN    4    NaN
Adding a new column using the existing columns in DataFrame:
   one  two  three  four
a  1.0    1   10.0  11.0
b  2.0    2   20.0  22.0
c  3.0    3   30.0  33.0
d  NaN    4    NaN   NaN
```

### DataFrame 删除列

下面演示 DataFrame 中如何删除或弹出一列

```python
# Using the previous DataFrame, we will delete a column
# using del function
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']), 
     'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd']), 
     'three' : pd.Series([10,20,30], index=['a', 'b', 'c']), 
     'four' : pd.Series([50,60,70,80], index=['a', 'b', 'c', 'e'])}
# 由此可以更加深刻地理解

df = pd.DataFrame(d)
print ("Our dataframe is:")
print (df)

# using del function
print ("Deleting the first column using DEL function:")
del df['one']
print (df)

# using pop function
print ("Deleting another column using POP function:")
df.pop('two')
print (df)

# using drop function
print ("Deleting another column using DROP function:")
df = df.drop(['three'], axis = 1)
print (df)

# using drop function
print ("Deleting another column using DROP function:")
df.drop(['four'], axis = 1, inplace = True)
print (df)
```

执行结果如下:

```shell
Our dataframe is:
   one  two  three  four
a  1.0  1.0   10.0  50.0
b  2.0  2.0   20.0  60.0
c  3.0  3.0   30.0  70.0
d  NaN  4.0    NaN   NaN
e  NaN  NaN    NaN  80.0
Deleting the first column using DEL function:
   two  three  four
a  1.0   10.0  50.0
b  2.0   20.0  60.0
c  3.0   30.0  70.0
d  4.0    NaN   NaN
e  NaN    NaN  80.0
Deleting another column using POP function:
   three  four
a   10.0  50.0
b   20.0  60.0
c   30.0  70.0
d    NaN   NaN
e    NaN  80.0
Deleting another column using DROP function:
   four
a  50.0
b  60.0
c  70.0
d   NaN
e  80.0
Deleting another column using DROP function:
Empty DataFrame
Columns: []
Index: [a, b, c, d, e]
```

### DataFrame 读取行

**按索引选择**
可以通过将行索引传递给`loc()`函数来选择行

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']), 
     'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print (df.loc['b'])
```

执行结果如下:

```shell
one    2.0
two    2.0
Name: b, dtype: float64
```

**按位置选择**
可以通过将整数位置传递给`iloc()`函数来选择行

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
     'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print (df.iloc[2])
```

执行结果如下:

```shell
one    3.0
two    3.0
Name: c, dtype: float64
```

**按行切片选择**
可以使用`:`运算符选择多行

```python
import pandas as pd

d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']), 
    'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print (df[2:4])
```

执行结果如下:

```shell
   one  two
c  3.0    3
d  NaN    4
```

**读取行列交叉项**

展示`iloc[]`的一些用法，`loc[]`同理。注意后面跟着的是`[]`不是`()`

```python
import pandas as pd
data = [[1,2,3],[4,5,6],[7,8,9]]
df = pd.DataFrame(data)
print("# df: ")
print(df)
print("# df.iloc[:,0:2]: ")
print(df.iloc[:,0:2])
print("# df.iloc[1:2,0:2]: ")
print(df.iloc[1:2,0:2])
print("# df.iloc[:,1]: ")
print(df.iloc[:,1])
print("# df.iloc[1]: ")
print(df.iloc[1])
print("# df.iloc[1:2]: ")
print(df.iloc[1:2])
print("# df.iloc[[1,2],[0,1]]: ")
print(df.iloc[[1,2],[0,1]])
```

执行结果如下:

```shell
# df: 
   0  1  2
0  1  2  3
1  4  5  6
2  7  8  9
# df.iloc[:,0:2]: 
   0  1
0  1  2
1  4  5
2  7  8
# df.iloc[1:2,0:2]: 
   0  1
1  4  5
# df.iloc[:,1]: 
0    2
1    5
2    8
Name: 1, dtype: int64
# df.iloc[1]: 
0    4
1    5
2    6
Name: 1, dtype: int64
# df.iloc[1:2]: 
   0  1  2
1  4  5  6
# df.iloc[[1,2],[0,1]]: 
   0  1
1  4  5
2  7  8
```

### DataFrame 添加行

使用`append()`函数将新行添加到 DataFrame

```python
import pandas as pd

df = pd.DataFrame([[1, 2], [3, 4]], columns = ['a','b'])
df2 = pd.DataFrame([[5, 6], [7, 8]], columns = ['a','b'])

df = df.append(df2)
print (df)
```

执行结果如下:

```shell
   a  b
0  1  2
1  3  4
0  5  6
1  7  8
```

### DataFrame 删除行

使用索引标签从 DataFrame 中删除行。 如果标签重复，则会删除多行。

```python
import pandas as pd

df = pd.DataFrame([[1, 2], [3, 4]], columns = ['a','b'])
df2 = pd.DataFrame([[5, 6], [7, 8]], columns = ['a','b'])

df = df.append(df2)

# Drop rows with label 0
df = df.drop(0)

print (df)
```

执行结果如下:

```shell
   a  b
1  3  4
1  7  8
```

> 在上面的例子中，一共有两行被删除，因为这两行包含相同的标签`0`。

**DataFrame 基本属性和方法**，前面介绍了[创建DataFrame](https://geek-docs.com/pandas/pandas-tutorials/pandas-create-dataframe.html)和[DataFrame的基本用法](https://geek-docs.com/pandas/pandas-tutorials/pandas-dataframe-read-add-delete.html)，下面来看看数据表格(DataFrame)的基本功能有哪些？下表列出了DataFrame的重要属性和方法。

![Pandas DataFrame 属性和方法](pandas%E6%B6%89%E7%8C%8E/201908302155.png)

下面来看看如何创建一个DataFrame并使用上述属性和方法。

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our data series is:")
print (df)
```

执行结果如下:

```shell
Our data series is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
```

### 改行列标题

```python
df.columns = ['name','gender','age'] #尽管我们只想把’sex’改为’gender’，但是仍然要把所有的列全写上，否则报错。
df.rename(columns = {'name':'Name','age':'Age'},inplace = True) #只修改name和age。inplace若为True，直接修改df，否则，不修改df，只是返回一个修改后的数据。
df.index = list('abc')#把index改为a,b,c.直接修改了df。
df.rename({1:'a',2:'b',3:'c'},axis = 0,inplace = True)#无返回值，直接修改df的index。
```

### shape 示例

返回表示`DataFrame`的维度的元组。 元组`(a，b)`，其中`a`表示行数，`b`表示列数。示例代码如下:

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our object is:")
print (df)
print ("The shape of the object is:")
print (df.shape)
```

执行结果如下:

```shell
Our object is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
The shape of the object is:
(7, 3)
```

### size 示例

返回 DataFrame 中的元素个数。示例代码如下:

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our object is:")
print (df)
print ("The total number of elements in our object is:")
print (df.size)
```

执行结果如下:

```shell
Our object is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
The total number of elements in our object is:
21
```

### values 示例

将`DataFrame`中的实际数据作为`NDarray`返回。示例代码如下:

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our object is:")
print (df)
print ("The actual data in our data frame is:")
print (df.values)
```

执行结果如下:

```shell
Our object is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
The actual data in our data frame is:
[[25 'Tom' 4.23]
 [26 'James' 3.24]
 [25 'Ricky' 3.98]
 [23 'Vin' 2.56]
 [30 'Steve' 3.2]
 [29 'Minsu' 4.6]
 [23 'Jack' 3.8]]
```

### head() 和 tail() 示例

要查看DataFrame对象的小样本，可使用`head()`和`tail()`方法。`head()`返回前`n`行(观察索引值)。默认数量为5，可以传递自定义数值。

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our data frame is:")
print (df)
print ("The first two rows of the data frame is:")
print (df.head(2))
```

执行结果如下:

```shell
Our data frame is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
The first two rows of the data frame is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
```

`tail()`返回最后`n`行(观察索引值)。默认数量为5，可以传递自定义数值。

```python
import pandas as pd
import numpy as np

#Create a Dictionary of series
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack']),
   'Age':pd.Series([25,26,25,23,30,29,23]), 
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8])}

#Create a DataFrame
df = pd.DataFrame(d)
print ("Our data frame is:")
print (df)
print ("The last two rows of the data frame is:")
print (df.tail(2))
```

执行结果如下:

```shell
Our data frame is:
   Age   Name  Rating
0   25    Tom    4.23
1   26  James    3.24
2   25  Ricky    3.98
3   23    Vin    2.56
4   30  Steve    3.20
5   29  Minsu    4.60
6   23   Jack    3.80
The last two rows of the data frame is:
   Age   Name  Rating
5   29  Minsu     4.6
6   23   Jack     3.8
```

### Tips

1. `axis=0/1`来确定行列，默认`axis=0`表示不跨行操作，`axis=1`表示跨行操作，而且需要自行指定。一般这个参数用在`append()`, `drop()`函数中。
2. 可以用`df.index`, `df.columns`, `df.values`查看`DataFrame`对象的行索引、列索引和数据。`df.values`实例见上文。
3. 可以用`df.T`查看行列转置后的`DataFrame`
4. `inplace=True`参数的说明: 用于`append()`和`drop()`函数，默认这个值为`False`表示不改变原有对象的值，修改原有对象值时需要对原有对象赋值（`df = df.drop("index1", axis=0)`）。设置为`True`后表示改变原有对象的值。
5. `df.at['a', '列2']`表示取第a行列2所对应的元素。
6. 注意: `df[2]`取列2，但是`df[2:4]`取行2~3。
7. `ignore_index=True`参数的说明: **未完待续** 

