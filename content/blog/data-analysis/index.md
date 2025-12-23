---
title: "Cai's Python数据分析笔记"
date: 2025-12-16T22:58:00+08:00
draft: false
tags: ["Python","数据分析","Numpy","Pandas","Matplotlib"]
---
## Numpy

一个超级强大的数学库，官方入门文档很详细

### 导入

导入`numpy`一般会用别名`np`，更短更好用，感觉numpy很多操作都是函数式的，所以用`np.`访问函数会简洁很多

```python
import numpy as np
```

### 创建数组

`array`是Numpy的核心数据类型，可以存入容易维度的各种类型的数组。

```python
# 使用np.array方法创建array
a = np.array([1, 2, 3, 4, 5, 6])

# 可以传入N维列表 (嵌套列表)
b = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

### 访问数组

使用`array`类型超强的索引器可以访问数组中的元素

```python
a[2] # np.int64(3)
b[2, 3] # np.int64(12)
b[2] # array([ 9, 10, 11, 12])
```

### 数组属性

```python
b.ndim # 2 数组的维度
b.shape # (3,4) 数组的形状，每个维度的长度
b.size # 12 数组的长度，元素数量
```

### 创建基本数组

#### `np.zeros`

创建元素均为0的数组

```python
np.zeros(2) # 创建1维长度为2元素均为0.的数组 (默认类型是float64)
'array([0., 0.])'

np.zeros(2, dtype='int64') # 指定元素类型为int64
'array([0, 0])'

np.zeros((3, 2, 5)) # 创建形状为(3, 2, 5)的数组
'''
array([[[0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.]],

       [[0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.]],

       [[0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.]]])
'''

```

#### `np.ones`

创建元素均为1的数组，和`zeros`行为相似

```python
np.ones(2)
'array([1., 1.])'

np.ones(2, dtype='int64')
'array([1, 1])'

np.ones((3, 2, 5))
'''
array([[[1., 1., 1., 1., 1.],
        [1., 1., 1., 1., 1.]],

       [[1., 1., 1., 1., 1.],
        [1., 1., 1., 1., 1.]],

       [[1., 1., 1., 1., 1.],
        [1., 1., 1., 1., 1.]]])
'''
```

#### `np.empty`

创建无初始元素的数组，所有元素都是随机值

```python
np.empty(2)
'array([9.99997383e-047, 4.58268884e+190])'

np.empty(2, dtype='int64')
'array([3918770264469437126, 7459247574283513330])'

np.empty((3, 2, 5))
'''
array([[[0.56538767, 0.17226228, 0.84961952, 0.8758129 , 0.09499191],
        [0.15787931, 0.54642934, 0.23798557, 0.3213524 , 0.4362137 ]],

       [[0.66447162, 0.38545947, 0.33029266, 0.78916451, 0.14392668],
        [0.81738665, 0.38102459, 0.51917035, 0.78983855, 0.04749948]],

       [[0.82287187, 0.89221285, 0.43368646, 0.88862339, 0.74596903],
        [0.98114036, 0.18744389, 0.21366053, 0.01748378, 0.79055294]]])
'''
```

#### `np.arange`

生成一个范围数组，可以指定步长和起始范围

```python
np.arange(5) # array([0, 1, 2, 3, 4])
np.arange(2, 9, 2) # array([2, 4, 6, 8])
```

#### `np.linspace`

生成一个等分范围的数组

```python
np.linspace(0, 10, num=5) # array([ 0. ,  2.5,  5. ,  7.5, 10. ])

np.linspace(0, 10) # 默认50等分
'''
array([ 0.        ,  0.20408163,  0.40816327,  0.6122449 ,  0.81632653,
        1.02040816,  1.2244898 ,  1.42857143,  1.63265306,  1.83673469,
        2.04081633,  2.24489796,  2.44897959,  2.65306122,  2.85714286,
        3.06122449,  3.26530612,  3.46938776,  3.67346939,  3.87755102,
        4.08163265,  4.28571429,  4.48979592,  4.69387755,  4.89795918,
        5.10204082,  5.30612245,  5.51020408,  5.71428571,  5.91836735,
        6.12244898,  6.32653061,  6.53061224,  6.73469388,  6.93877551,
        7.14285714,  7.34693878,  7.55102041,  7.75510204,  7.95918367,
        8.16326531,  8.36734694,  8.57142857,  8.7755102 ,  8.97959184,
        9.18367347,  9.3877551 ,  9.59183673,  9.79591837, 10.        ])
'''
```

### 排序、连接、切分

#### `np.sort`

从小到大排序排序，并返回一个新数组

```python
arr = np.array([2, 1, 5, 3, 7, 4, 6, 8])
np.sort(arr) # array([1, 2, 3, 4, 5, 6, 7, 8])

arr2 = np.array([[2, 1, 3],[4,6,5],[9,8,7]])
np.sort(arr2)
'''
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
'''
```

#### `np.argsort`

返回排序后的索引

```python
np.argsort(arr) # array([1, 0, 3, 5, 2, 6, 4, 7])
```

#### `np.concatenate`

拼接两个数组

```python
a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])
np.concatenate((a, b)) # array([1, 2, 3, 4, 5, 6, 7, 8])

x = np.array([[1, 2], [3, 4]])
y = np.array([[5, 6]])
np.concatenate((x, y), axis=0) # x轴方向上

'''
array([[1, 2],
       [3, 4],
       [5, 6]])
'''
```

#### `np.vstack`

垂直连接数组

```python
a1 = np.array([[1, 1],
               [2, 2]])

a2 = np.array([[3, 3],
               [4, 4]])

np.vstack((a1, a2))
'''
array([[1, 1],
       [2, 2],
       [3, 3],
       [4, 4]]
'''
```

#### `np.hstack`

水平连接数组

```python
np.hstack((a1, a2))
'''
array([[1, 1, 3, 3],
       [2, 2, 4, 4]])
'''
```

#### `np.hsplit`

水平分割数组

```python
x = np.arange(1, 25).reshape(2, 12)
'''
array([[ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12],
       [13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24]])
'''

np.hsplit(x, 3) # 分成均3份
'''
  [array([[ 1,  2,  3,  4],
         [13, 14, 15, 16]]), array([[ 5,  6,  7,  8],
         [17, 18, 19, 20]]), array([[ 9, 10, 11, 12],
         [21, 22, 23, 24]])]
'''

np.hsplit(x, (3, 4)) # 从3,4前切开
'''
  [array([[ 1,  2,  3],
         [13, 14, 15]]), array([[ 4],
         [16]]), array([[ 5,  6,  7,  8,  9, 10, 11, 12],
         [17, 18, 19, 20, 21, 22, 23, 24]])]
'''
       
```

#### `np.vsplit`

垂直分割数组

```python
x = np.arange(1, 25).reshape(6, 4)
'''
array([[ 1,  2,  3,  4],
       [ 5,  6,  7,  8],
       [ 9, 10, 11, 12],
       [13, 14, 15, 16],
       [17, 18, 19, 20],
       [21, 22, 23, 24]])
'''

np.vsplit(x, 3) # 分成均3份
'''
  [array([[ 1,  2,  3,  4],
         [ 5,  6,  7,  8]]),
   array([[ 9, 10, 11, 12],
         [13, 14, 15, 16]]),
   array([[17, 18, 19, 20],
         [21, 22, 23, 24]])]
'''

np.vsplit(x, (3, 4)) # 从3,4前切开
'''
  [array([[ 1,  2,  3,  4],
         [ 5,  6,  7,  8],
         [ 9, 10, 11, 12]]),
   array([[13, 14, 15, 16]]),
   array([[17, 18, 19, 20],
         [21, 22, 23, 24]])]
'''
```

### 重塑数组

将数组重塑为指定的形状

```python
a = np.arange(6) # array([0, 1, 2, 3, 4, 5])
a.reshape(2, 3)
'''
array([[0, 1, 2],
       [3, 4, 5]])
'''
```

### 添加轴

#### `np.newaxis`

用来表示新的轴

```python
a = np.array([1, 2, 3, 4, 5, 6])
a[np.newaxis, :] # 向前添加轴
'array([[1, 2, 3, 4, 5, 6]])' # (1, 6)


a[: ,np.newaxis] # 向后添加轴
'''
array([[1], # (6, 1)
       [2],
       [3],
       [4],
       [5],
       [6]])
'''
```

#### `np.expand_dims`

```python
a = np.array([1, 2, 3, 4, 5, 6])
np.expand_dims(a, axis=0) # 向前添加轴
'array([[1, 2, 3, 4, 5, 6]])' # (1, 6)


np.expand_dims(a, axis=-1) # 向后添加轴，负索引
np.expand_dims(a, axis=1) # 同上
'''
array([[1], # (6, 1)
       [2],
       [3],
       [4],
       [5],
       [6]])
```

### 索引和切片

数组具有强大的索引器

```python
data = np.array([1, 2, 3])

data[0] # 访问元素
'np.int64(1)'

data[0,2] # 切片操作，返回视图
'array([1, 2])'

data >= 2 # 布尔掩码
'array([False,  True,  True])'

data[data>=2] # 使用布尔掩码获取满足条件的元素
'array([2, 3])'

data[(data>=2)&(data%2!=0)] # 并列条件
'array([3])'

data[(data==3)|(data==1)] # 或条件
'array([1, 3])'

np.nonzero(data>=2) # 利用np.nonzero获取索引
'(array([1, 2]),)'
```

### 视图和复制

```python
a = np.array([[1, 2, 3], [4, 5, 6]])

a.view() # 视图，改动会同步到原数组
a.copy() # 复制，完全独立
```

### 数组基本操作

#### 四则运算

```python
a = np.array([4, 2])
b = np.array([2, 2])

a+b # array([6, 4])
a-b # array([2, 0])
a*b # array([8, 4])
a/b # array([2., 1.])
a//b # array([2, 1])
a@b # np.int64(12)
```

#### 统计

```python
c = np.array([[1, 1], [2, 2]])
c.sum() # np.int64(6) 求和
c.max() # np.int64(2) 最大
c.min() # np.int64(1) 最小
c.mean() # np.float64(1.5) 平均

c.sum(axis=1) # array([2, 4])
c.max(axis=1) # array([1, 2])
c.min(axis=1) # array([1, 2])
c.mean(axis=1) # array([1., 2.])

np.unique(c) # array([1, 2]) 去重元素
np.unique(c, return_index=True) # array([1, 2]), array([0, 2])

```

#### 广播

允许不同形状的数组进行计算

1. 规则1：如果数组维度不同，在较小数组的形状前面加 1
2. 规则2：如果数组在某个维度上大小不同，且其中一个为1，则可以广播
3. 规则3：如果都不满足，则报错

```python
a = np.array([4, 2])
a*2 # array([8, 4])

A = np.array([[1, 2, 3],      # 形状: (2, 3)
              [4, 5, 6]])

B = np.array([10, 20, 30])    # 形状: (3,)

# B会自动广播成 [[10, 20, 30],
#               [10, 20, 30]]

A+B
'''
array([[11, 22, 33],
       [14, 25, 36]])
'''

```

### 反转数组

```python
a = np.arange(8)

np.flip(a) # array([7, 6, 5, 4, 3, 2, 1, 0])
a[::-1] # array([7, 6, 5, 4, 3, 2, 1, 0])
```

> `a[start:stop:step]`，所以`a[::-1]`会直接反转

### 转置数组

```python
a = np.array([[1, 2, 3],
              [4, 5, 6]])

a.transpose()
'''
array([[1, 4],
       [2, 5],
       [3, 6]])
'''

a.T
'''
array([[1, 4],
       [2, 5],
       [3, 6]])
'''

```

### 展开数组

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

arr.flatten() # 拷贝数组

arr.ravel() # 视图
```

### 生成随机数

```python
rng = np.random.default_rng()

rng.random() # 生成随机数
'0.5498227837461795'

rng.integers(low=0, high=10, size=5) # 生成随机整数
'''
array([[[6, 2, 2, 9],
        [7, 9, 0, 1],
        [6, 4, 3, 8]],

       [[6, 8, 0, 3],
        [3, 8, 7, 2],
        [4, 4, 3, 7]]])
'''

```

## Pands

官方入门文档学的一丢丢

### 导入

和numpy差不多，推荐用缩写

```python
import numpy as np
import pandas as pd
```

> 苦命鸳鸯

### Series

`Series`是一种一维的标记数组，相当于表格的列

```python
s = pd.Series(data, index=index)
```

这里的`data`可以是不同的东西:

- ndarray
- Python字典
- 标量

`index`是这个`Series`的坐标轴标签，取决于`data`

> Pandas支持非唯一索引，但有一些操作不支持，如果对含非唯一索引的Series执行这些操作则会抛出异常

### DataFrame

`DataFrame`是二维标记数组，有多个列，每个列可以有不同的数据结构，和SQL Table类似，在Pandas里比较常用

```python
dates = pd.date_range("20130101", periods=6)

df = pd.DataFrame(
    {
        "A": 1.0,
        "B": pd.Timestamp("20130102"),
        "C": pd.Series(1, index=list(range(4)), dtype="float32"),
        "D": np.array([3] * 4, dtype="int32"),
        "E": pd.Categorical(["test", "train", "test", "train"]),
        "F": "foo",
    }
)
```

可以由以下东西创建：

- 一维数组字典、多个list、多个dict、多个`Series`
- 二维数组
- 结构化或记录数组
- 一个`Series`
- 其他`DataFrame`

### 查看数据

#### `head`

查看顶部几行的数据，有点像SQL的`LIMIT`

```python
df.head(2)
'''
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
'''
```

#### `tail`

查看尾部几行的数据

```python
df.tail(2)
'''
     A          B    C  D      E    F
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
'''
```

#### `to_numpy`

把`DataFrame`转为`ndarray`

```python
df.to_numpy()
'''
array([[1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo']],
      dtype=object)
'''
```

#### `describe`

查看统计摘要

```python
df.describe()
'''
         A                    B    C    D
count  4.0                    4  4.0  4.0
mean   1.0  2013-01-02 00:00:00  1.0  3.0
min    1.0  2013-01-02 00:00:00  1.0  3.0
25%    1.0  2013-01-02 00:00:00  1.0  3.0
50%    1.0  2013-01-02 00:00:00  1.0  3.0
75%    1.0  2013-01-02 00:00:00  1.0  3.0
max    1.0  2013-01-02 00:00:00  1.0  3.0
std    0.0                  NaN  0.0  0.0
```

#### `T`

转置`DataFrame`

```python
df.T
'''
                     0                    1                    2                    3
A                  1.0                  1.0                  1.0                  1.0
B  2013-01-02 00:00:00  2013-01-02 00:00:00  2013-01-02 00:00:00  2013-01-02 00:00:00
C                  1.0                  1.0                  1.0                  1.0
D                    3                    3                    3                    3
E                 test                train                 test                train
F                  foo                  foo                  foo                  foo
```

#### `sort_index`

就是按照指定轴上的标签进行排序，其中`axis 0`是行轴，`axis 1`是列轴，`axis`参数表示沿着某个轴操作，`ascending`参数表示使用升序排序。

```text
    列方向 → axis=1（水平）
    ┌─────────────┐
行  │             │
方  │             │
向  │   数组/表    │ axis=0
↓   │             │（垂直）
    │             │
    └─────────────┘
```

```python
df.sort_index(axis=1, ascending=False)
'''
     F      E  D    C          B    A
0  foo   test  3  1.0 2013-01-02  1.0
1  foo  train  3  1.0 2013-01-02  1.0
2  foo   test  3  1.0 2013-01-02  1.0
3  foo  train  3  1.0 2013-01-02  1.0
'''

df.sort_index(axis=0, ascending=False)
'''
     A          B    C  D      E    F
3  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
0  1.0 2013-01-02  1.0  3   test  foo
'''
```

#### `sort_values`

按照指定列排序行

```python
df.sort_values("E", ascending=True)
'''
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
2  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
3  1.0 2013-01-02  1.0  3  train  foo
'''
```

### 选择数据

| 方法          | 选择类型 | 索引方式           | 输入类型                     | 主要用途                         |
|---------------|----------|--------------------|------------------------------|----------------------------------|
| `df.at[]`     | 单值     | 标签（行列名）     | 单个行标签, 单个列标签       | 极速获取/设置单个标量值      |
| `df.iat[]`    | 单值     | 整数位置           | 单个行索引, 单个列索引       | 极速获取/设置单个标量值（按位置）|
| `df.loc[]`    | 单/多值  | 标签（行列名）     | 单个标签、列表、切片、布尔索引  | 基于标签的索引和切片 |
| `df.iloc[]`   | 单/多值  | 整数位置           | 单个整数、列表、切片、布尔索引 | 基于整数位置的索引和切片  |

#### 索引器 (奇葩)

使用一个列标签来选择一个列，返回`Series`

```python
df['B']
'''
0   2013-01-02
1   2013-01-02
2   2013-01-02
3   2013-01-02
Name: B, dtype: datetime64[s]
'''
```

使用切片`:`获取范围内的行

```python
df[:2]
'''
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
'''
```

#### 标签选择

使用行标签选择，返回该行的`Series`

```python
df.loc[0]
'''
A                    1.0
B    2013-01-02 00:00:00
C                    1.0
D                      3
E                   test
F                    foo
Name: 0, dtype: object
'''
```

使用`:`选择所有行，并且选择列 (有点像SQL的`SELECT column`)

```python
df.loc[:,["A", "F"]]
'''
     A    F
0  1.0  foo
1  1.0  foo
2  1.0  foo
3  1.0  foo
'''
```

选择范围内的行的指定列

```python
df.loc[2:3,["A", "F"]]
'''
     A    F
2  1.0  foo
3  1.0  foo
'''
```

选择一行和一列，返回该位置的元素

```python
df.loc[0, "A"] # 通用性更强
'''
np.float64(1.0)
'''

df.at[0, "A"] # 性能更好
'''
np.float64(1.0)
'''
```

#### 索引选择

使用行索引选择，返回该行的`Series`

```python
df.iloc[1]
'''
A                    1.0
B    2013-01-02 00:00:00
C                    1.0
D                      3
E                  train
F                    foo
Name: 1, dtype: object
'''
```

使用`:`选择所有行，并且选择列 (有点像SQL的`SELECT column`)

```python
df.iloc[:, [0, 4]]
'''
     A      E
0  1.0   test
1  1.0  train
2  1.0   test
3  1.0  trai
'''
```

选择范围内的行的指定列

```python
df.iloc[2:3, [0, 4]]
'''
     A     E
2  1.0  test
'''
```

选择一行和一列，返回该位置的元素

```python
df.iloc[0, 0] # 通用性更强
'''
np.float64(1.0)
'''

df.iat[0, 0] # 性能更好
'''
np.float64(1.0)
'''
```

> 其实就是把标签换成了索引，然后开头加个`i`，用法完全一样

#### 条件选择

和numpy那套是基本一样的

```python
df[df["E"] == "test"]
'''
     A          B    C  D     E    F
0  1.0 2013-01-02  1.0  3  test  foo
2  1.0 2013-01-02  1.0  3  test  foo
'''
```

使用`isin()`函数来过滤

```python
df[df["E"].isin(["train"])]
'''
     A          B    C  D      E    F
1  1.0 2013-01-02  1.0  3  train  foo
3  1.0 2013-01-02  1.0  3  train  foo
'''
```

### 缺失数据

对于numpy的数据类型，使用`np.nan`表示缺失数据，缺失数据不参与运算

```python
dates = pd.date_range("20130101", periods=6)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ["E"])
df1.loc[dates[0] : dates[1], "E"] = 1
'''
                   A         B         C         D    E
2013-01-01  0.235301 -1.447165 -0.568950  0.132561  1.0
2013-01-02  0.647259 -0.742841  0.462939  1.630470  1.0
2013-01-03 -1.692941  2.993959 -0.554808  0.090538  NaN
2013-01-04  0.831446  0.669407  0.916880  0.213223  NaN
'''
```

#### `dropna`

删除所有含缺失数据的行

```python
df1.dropna()
'''
                   A         B         C         D    E
2013-01-01  0.235301 -1.447165 -0.568950  0.132561  1.0
2013-01-02  0.647259 -0.742841  0.462939  1.630470  1.0
'''
```

#### `fillna`

填充所有缺失数据

```python
df1.fillna(0)
'''
                   A         B         C         D    E
2013-01-01  0.235301 -1.447165 -0.568950  0.132561  1.0
2013-01-02  0.647259 -0.742841  0.462939  1.630470  1.0
2013-01-03 -1.692941  2.993959 -0.554808  0.090538  0.0
2013-01-04  0.831446  0.669407  0.916880  0.213223  0.0
'''
```

#### `isna`

获取缺失数据位置的掩码

```python
df1.isna(0)
'''
                A      B      C      D      E
2013-01-01  False  False  False  False  False
2013-01-02  False  False  False  False  False
2013-01-03  False  False  False  False   True
2013-01-04  False  False  False  False   True
'''
```

### 操作

#### 统计

计算列的平均值

```python

df.mean()
'''
A    0.435725
B   -0.043070
C    0.082099
D    0.023473
dtype: float64
'''
```

计算每行的平均值

```python
df.mean(axis=1)
'''

2013-01-01   -0.412063
2013-01-02    0.499457
2013-01-03    0.209187
2013-01-04    0.657739
2013-01-05    0.190225
2013-01-06   -0.397203
Freq: D, dtype: float64
'''
```

#### 用户自定义函数

`DataFrame.agg()` 和 `DataFrame.transform()` 分别通过用户自定义函数实现数据的归约聚合或广播转换。`DataFrame.agg()`主要用于聚合数据，其实也可以转化数据，可以输出任何形状的数据，`DataFrame.transform()`严格要求输出和输入的数据形状一致

```python
df.agg(lambda x: np.mean(x) * 114514)
'''
A    49896.638512
B    -4932.089916
C     9401.504045
D     2687.983148
dtype: float64
'''

df.agg(lambda x: x // 2)
'''
              A    B    C    D
2013-01-01  0.0 -1.0 -1.0  0.0
2013-01-02  0.0 -1.0  0.0  0.0
2013-01-03 -1.0  1.0 -1.0  0.0
2013-01-04  0.0  0.0  0.0  0.0
2013-01-05  0.0  0.0 -1.0  0.0
2013-01-06  0.0 -1.0  0.0 -2.0
'''

df.transform(lambda x: x // 2)
'''
              A    B    C    D
2013-01-01  0.0 -1.0 -1.0  0.0
2013-01-02  0.0 -1.0  0.0  0.0
2013-01-03 -1.0  1.0 -1.0  0.0
2013-01-04  0.0  0.0  0.0  0.0
2013-01-05  0.0  0.0 -1.0  0.0
2013-01-06  0.0 -1.0  0.0 -2.0
'''


```

#### 计数

```python
s = pd.Series(np.random.randint(0, 7, size=10))
s.value_counts()
'''
5    3
4    2
2    2
3    1
6    1
1    1
Name: count, dtype: int64
'''
```

#### 字符串方法

```python
s = pd.Series(["A", "B", "C", "Aaba", "Baca", np.nan, "CABA", "dog", "cat"])
s.str.upper()
'''
0       A
1       B
2       C
3    AABA
4    BACA
5     NaN
6    CABA
7     DOG
8     CAT
dtype: object
'''

s.str.lower()
'''
0       a
1       b
2       c
3    aaba
4    baca
5     NaN
6    caba
7     dog
8     cat
dtype: object
'''

s.str.len()
'''
0    1.0
1    1.0
2    1.0
3    4.0
4    4.0
5    NaN
6    4.0
7    3.0
8    3.0
dtype: float6
'''
```

### 合并

#### 拼接

将pandas对象按行拼接在一起

```python
df = pd.DataFrame(np.random.randn(10, 4))
'''
          0         1         2         3
0 -0.837824 -1.167169 -0.618485 -1.246306
1 -1.118622  0.411848  0.254821  1.378062
2 -0.335262 -0.914664  0.872622 -0.027071
3 -1.138401 -0.958185 -1.812294 -0.553423
4  0.102264  0.699800 -0.777422  0.200424
5  0.758224 -0.074789  1.038832  1.256439
6 -1.703866 -1.613236 -1.739121  0.660869
7 -0.202777 -0.518916  0.288470  0.181069
8  0.667791 -0.558353 -0.907524  0.224313
9 -0.384725  0.256703  1.386798 -1.492503
'''

pieces = [df[:3], df[3:7], df[7:]]
'''
[          0         1         2         3
0 -0.837824 -1.167169 -0.618485 -1.246306
1 -1.118622  0.411848  0.254821  1.378062
2 -0.335262 -0.914664  0.872622 -0.027071,           0         1         2         3
3 -1.138401 -0.958185 -1.812294 -0.553423
4  0.102264  0.699800 -0.777422  0.200424
5  0.758224 -0.074789  1.038832  1.256439
6 -1.703866 -1.613236 -1.739121  0.660869,           0         1         2         3
7 -0.202777 -0.518916  0.288470  0.181069
8  0.667791 -0.558353 -0.907524  0.224313
9 -0.384725  0.256703  1.386798 -1.492503]
'''

pd.concat(pieces)
'''
          0         1         2         3
0 -0.837824 -1.167169 -0.618485 -1.246306
1 -1.118622  0.411848  0.254821  1.378062
2 -0.335262 -0.914664  0.872622 -0.027071
3 -1.138401 -0.958185 -1.812294 -0.553423
4  0.102264  0.699800 -0.777422  0.200424
5  0.758224 -0.074789  1.038832  1.256439
6 -1.703866 -1.613236 -1.739121  0.660869
7 -0.202777 -0.518916  0.288470  0.181069
8  0.667791 -0.558353 -0.907524  0.224313
9 -0.384725  0.256703  1.386798 -1.492503
'''

```

#### 连接

像使用SQL的`JOIN`一样把数据数据按列连接

```python
left = pd.DataFrame({"key": ["foo", "foo"], "lval": [1, 2]})
'''
   key  lval
0  foo     1
1  foo     
'''

right = pd.DataFrame({"key": ["foo", "foo"], "rval": [4, 5]})
'''
   key  rval
0  foo     4
1  foo     5
'''

pd.merge(left, right, on="key")
'''
   key  lval  rval
0  foo     1     4
1  foo     1     5
2  foo     2     4
3  foo     2     5
'''
```

当`key`唯一时`merge`

```python
left = pd.DataFrame({"key": ["foo", "bar"], "lval": [1, 2]})
'''
   key  lval
0  foo     1
1  bar     2
'''

right = pd.DataFrame({"key": ["foo", "bar"], "rval": [4, 5]})
'''
   key  rval
0  foo     4
1  bar     5
'''

pd.merge(left, right, on="key")
'''
   key  lval  rval
0  foo     1     4
1  bar     2     5
'''
```

### 分组

分组一般由这几步:

- 按照规则把数据分为几组
- 对每一个组使用对于的函数
- 将结果合并到原数据中

```python
df = pd.DataFrame(
    {
        "A": ["foo", "bar", "foo", "bar", "foo", "bar", "foo", "foo"],
        "B": ["one", "one", "two", "three", "two", "two", "one", "three"],
        "C": np.random.randn(8),
        "D": np.random.randn(8),
    }
)
'''
     A      B         C         D
0  foo    one  1.744921  1.099655
1  bar    one  1.048275  0.864212
2  foo    two  0.447586 -1.856555
3  bar  three -0.404339 -1.602069
4  foo    two  0.506537 -0.876018
5  bar    two  0.518548  1.293865
6  foo    one -0.358148  0.547363
7  foo  three -0.622471  0.287637
'''
```

按照列标签分组并且选择对应的列标签，然后应用方法

```python
df.groupby("A")[["C","D"]].apply(lambda x: x*2)
'''
              C         D
A
bar 1  2.096550  1.728425
    3 -0.808677 -3.204137
    5  1.037096  2.587729
foo 0  3.489842  2.199309
    2  0.895171 -3.713110
    4  1.013073 -1.752035
    6 -0.716297  1.094725
    7 -1.244942  0.575274
'''

df.groupby("A")[["C","D"]].sum()
'''
            C         D
A
bar  1.162485  0.556008
foo  1.718424 -0.797919
'''
```

按照多个标签分组

```python
'''
                  C         D
A   B
bar one    1.048275  0.864212
    three -0.404339 -1.602069
    two    0.518548  1.293865
foo one    1.386773  1.647017
    three -0.622471  0.287637
    two    0.954122 -2.732573
'''
```

### 时间序列

pandass有简单、强大、高校的方法来执行重采样操作

```python
rng = pd.date_range("1/1/2012", periods=100, freq="s")
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)

'''
2012-01-01 00:00:00    160
2012-01-01 00:00:01    463
                      ...
2012-01-01 00:01:38    131
2012-01-01 00:01:39    352
Freq: s, Length: 100, dtype: int32
'''

# 按照50秒的固定频率重新划分时间窗口，并对每个窗口内的所有值进行求和
ts.resample("50s").sum() 
'''
2012-01-01 00:00:00    12208
2012-01-01 00:00:50    11475
Freq: 50s, dtype: int32
'''

```python
rng = pd.date_range("3/6/2012 00:00", periods=5, freq="D")
ts = pd.Series(np.random.randn(len(rng)), rng)
'''
2012-03-06    1.721395
2012-03-07   -1.420167
2012-03-08    0.526491
2012-03-09    0.045274
2012-03-10   -1.946269
Freq: D, dtype: float64
'''

ts_utc = ts.tz_localize("UTC") # 设为UTC时间
'''
2012-03-06 00:00:00+00:00    1.721395
2012-03-07 00:00:00+00:00   -1.420167
2012-03-08 00:00:00+00:00    0.526491
2012-03-09 00:00:00+00:00    0.045274
2012-03-10 00:00:00+00:00   -1.946269
Freq: D, dtype: float64
'''

ts_utc.tz_convert("Asia/Shanghai") # 转为上海时区
'''
2012-03-06 08:00:00+08:00    1.721395
2012-03-07 08:00:00+08:00   -1.420167
2012-03-08 08:00:00+08:00    0.526491
2012-03-09 08:00:00+08:00    0.045274
2012-03-10 08:00:00+08:00   -1.946269
Freq: D, dtype: float64
'''

ts_utc.index + pd.offsets.BusinessDay(5) # 加上5个工作日
'''
DatetimeIndex(['2012-03-13 00:00:00+00:00', '2012-03-14 00:00:00+00:00',
               '2012-03-15 00:00:00+00:00', '2012-03-16 00:00:00+00:00',
               '2012-03-16 00:00:00+00:00'],
              dtype='datetime64[ns, UTC]', freq=None)
'''
```

### 分类

pandas可以在`DataFrame`中包含分类数据

```python
df = pd.DataFrame(
    {"id": [1, 2, 3, 4, 5, 6], "raw_grade": ["a", "b", "b", "a", "a", "e"]}
)
'''
   id raw_grade
0   1         a
1   2         b
2   3         b
3   4         a
4   5         a
5   6         e
'''

# 把原始等级转换为分类类型
df["grade"] = df ["raw_grade"].astype("category")
'''
   id raw_grade grade
0   1         a     a
1   2         b     b
2   3         b     b
3   4         a     a
4   5         a     a
5   6         e     e
'''

df["grade"]
'''
0    a
1    b
2    b
3    a
4    a
5    e
Name: grade, dtype: category
Categories (3, object): ['a', 'b', 'e']
'''

new_categories = ["very good", "good", "very bad"]

df["grade"] = df["grade"].cat.rename_categories(new_categories)

df["grade"] = df["grade"].cat.set_categories(
    ["very bad", "bad", "medium", "good", "very good"]
)
'''
   id raw_grade      grade
0   1         a  very good
1   2         b       good
2   3         b       good
3   4         a  very good
4   5         a  very good
5   6         e   very bad
'''

df.sort_values(by="grade")
'''
   id raw_grade      grade
5   6         e   very bad
1   2         b       good
2   3         b       good
0   1         a  very good
3   4         a  very good
4   5         a  very good
'''

```

### 画图

```python
import matplotlib.pyplot as plt

ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))
ts = ts.cumsum()
ts.plot() # 重头戏
plt.show()
```

### 导入和导出数据

使用`read_xxx`导入数据，`to_xxx`导出数据

```python
df = pd.DataFrame(np.random.randint(0, 5, (10, 5)))
df.to_csv("foo.csv")

pd.read_csv("foo.csv")
'''
   Unnamed: 0  0  1  2  3  4
0           0  4  3  1  1  2
1           1  1  0  2  3  2
2           2  1  4  2  1  2
3           3  0  4  0  2  2
4           4  4  2  2  3  4
5           5  4  0  4  3  1
6           6  2  1  2  0  3
7           7  4  0  4  4  4
8           8  4  4  1  0  1
9           9  0  4  3  0  3
'''
```

## Matplotlib

cosplay matlab的绘图库

### 导入

```python
import matplotlib.pyplot as plt
import numpy as np
```

### 简单示例

```python
fig, ax = plt.subplots()             # 创建一个只有一个绘图区的图布
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])  # 在绘图区绘制数据
plt.show()                           # 展示图形
```

### 图组成

![img](https://matplotlib.org/stable/_images/anatomy.png)

#### `Figure`

整个图保持追踪所有特殊的子元素`Axes`，还有嵌套的子图

#### `Axes`

`Axes`是附在`Figure`上的一个画图区域，包含多个`Axis`对象。可以使用`set_title()`设置标题，`set_xlabel()`和`set_ylabel`设置坐标轴标签等

#### `Axis`

`Axis`是附在`Axes`上的一个轴

#### `Artist`

图上的元素，任何在图上可以看见的东西

### 绘图方法的输入类型

绘图方法需要`np.array`、`np.ma.masked_array`或者可以被转为`np.array`的对象作为输入

```python
b = np.matrix([[1, 2], [3, 4]])
b_asarray = np.asarray(b) # 将对象转为ndarray

data = {'a': np.arange(50),
        'c': np.random.randint(0, 50, 50),
        'd': np.random.randn(50)}
data['b'] = data['a'] + 10 * np.random.randn(50)
data['d'] = np.abs(data['d']) * 100

fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained') # 设置图的尺寸(宽，高，英寸)，布局为自适应
ax.scatter('a', 'b', c='c', s='d', data=data) # 绘制散点图
ax.set_xlabel('entry a') # 设置X轴标签
ax.set_ylabel('entry b') # 设置Y轴标签
```

### 元素样式

大多数绘图方法有样式选项

```python
data1, data2, data3, data4 = np.random.randn(4, 100)

fig, ax = plt.subplots(figsize=(5, 2.7))
x = np.arange(len(data1))
ax.plot(x, np.cumsum(data1), color='blue', linewidth=3, linestyle='--') # 累加计算，蓝色，线宽3，虚线
l, = ax.plot(x, np.cumsum(data2), color='orange', linewidth=2) # 这里返回线对象的列表，用,是为了解包
l.set_linestyle(':') # 设为点线
```

#### 颜色

```python
fig, ax = plt.subplots(figsize=(5, 2.7))
ax.scatter(data1, data2, s=50, facecolor='C0', edgecolor='k') # 颜色设为C0(第一个默认颜色)，边框设为k(黑色)
```

#### 线宽、线样式和标记样式

```python
fig, ax = plt.subplots(figsize=(5, 2.7))
ax.plot(data1, 'o', label='data1') # 圆形标记
ax.plot(data2, 'd', label='data2') # 菱形标记
ax.plot(data3, 'v', label='data3') # 倒三角形标记
ax.plot(data4, 's', label='data4') # 正方形标记
ax.legend()
```

### 标签绘制

#### 轴、标签和文本

```python
mu, sigma = 115, 15
x = mu + sigma * np.random.randn(10000)
fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')

n, bins, patches = ax.hist(x, 50, density=True, facecolor='C0', alpha=0.75) # 绘制直方图 (数据，柱子数量，归一化概率密度，填充色，透明度)

ax.set_xlabel('Length [cm]') # X轴标签
ax.set_ylabel('Probability') # Y轴标签
ax.set_title('Aardvark lengths\n (not really)') # 标题
ax.text(75, .025, r'$\mu=115,\ \sigma=15$') # 绘制文本
ax.axis((55, 175, 0, 0.03)) # 设置轴的显示范围 [x最小值, x最大值, y最小值, y最大值]
ax.grid(True) # 启用网格
```

#### 使用数学公式

```python
ax.set_title(r'$\sigma_i=15$') # LaTeX格式
```

#### 注释

```python
fig, ax = plt.subplots(figsize=(5, 2.7))

t = np.arange(0.0, 5.0, 0.01)
s = np.cos(2 * np.pi * t)
line, = ax.plot(t, s, lw=2)

ax.annotate('local max', xy=(2, 1), xytext=(3, 1.5), # 文本 标注坐标 文本坐标
            arrowprops=dict(facecolor='black', shrink=0.05)) # 箭头 填充色 末端收缩量(?)

ax.set_ylim(-2, 2) # 设置Y轴显示范围
```

#### 图例

```python
fig, ax = plt.subplots(figsize=(5, 2.7))
ax.plot(np.arange(len(data1)), data1, label='data1')
ax.plot(np.arange(len(data2)), data2, label='data2')
ax.plot(np.arange(len(data3)), data3, 'd', label='data3')
ax.legend() # 显示图例
```

### 轴刻度和刻度线

```python
fig, axs = plt.subplots(1, 2, figsize=(5, 2.7), layout='constrained')
xdata = np.arange(len(data1))
data = 10**data1
axs[0].plot(xdata, data)

axs[1].set_yscale('log') # Y轴刻度设置为对数坐标
axs[1].plot(xdata, data)
```

#### 刻度定位器和格式化器

```python
fig, axs = plt.subplots(2, 1, layout='constrained')
axs[0].plot(xdata, data1)
axs[0].set_title('Automatic ticks')

axs[1].plot(xdata, data1)
axs[1].set_xticks(np.arange(0, 100, 30), ['zero', '30', 'sixty', '90']) # 设置X坐标轴刻度
axs[1].set_yticks([-1.5, 0, 1.5])  # 设置Y坐标轴刻度
axs[1].set_title('Manual ticks')
```

#### 绘制日期和字符串

```python
from matplotlib.dates import ConciseDateFormatter

fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')
dates = np.arange(np.datetime64('2021-11-15'), np.datetime64('2021-12-25'),
                  np.timedelta64(1, 'h'))
data = np.cumsum(np.random.randn(len(dates)))
ax.plot(dates, data)
ax.xaxis.set_major_formatter(ConciseDateFormatter(ax.xaxis.get_major_locator())) # 设置简洁的日期时间格式化器
# set_major_formatter设置格式化器
# get_major_locator()获取主轴刻度位置

fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')
categories = ['turnips', 'rutabaga', 'cucumber', 'pumpkins']

ax.bar(categories, np.random.rand(len(categories))) # 条形图
```

#### 附加 Axis 对象

```python
fig, (ax1, ax3) = plt.subplots(1, 2, figsize=(7, 2.7), layout='constrained')
l1, = ax1.plot(t, s)
ax2 = ax1.twinx() # 创建共享X轴的第二个Y轴
l2, = ax2.plot(t, range(len(t)), 'C1')
ax2.legend([l1, l2], ['Sine (left)', 'Straight (right)'])

ax3.plot(t, s)
ax3.set_xlabel('Angle [rad]')
ax4 = ax3.secondary_xaxis('top', (np.rad2deg, np.deg2rad)) # 创建第二个x轴
ax4.set_xlabel('Angle [°]')
```

### 颜色映射数据

```python
from matplotlib.colors import LogNorm

X, Y = np.meshgrid(np.linspace(-3, 3, 128), np.linspace(-3, 3, 128))
Z = (1 - X/2 + X**5 + Y**3) * np.exp(-X**2 - Y**2)

fig, axs = plt.subplots(2, 2, layout='constrained')
pc = axs[0, 0].pcolormesh(X, Y, Z, vmin=-1, vmax=1, cmap='RdBu_r')
fig.colorbar(pc, ax=axs[0, 0])
axs[0, 0].set_title('pcolormesh()')

co = axs[0, 1].contourf(X, Y, Z, levels=np.linspace(-1.25, 1.25, 11))
fig.colorbar(co, ax=axs[0, 1])
axs[0, 1].set_title('contourf()')

pc = axs[1, 0].imshow(Z**2 * 100, cmap='plasma', norm=LogNorm(vmin=0.01, vmax=100))
fig.colorbar(pc, ax=axs[1, 0], extend='both')
axs[1, 0].set_title('imshow() with LogNorm()')

pc = axs[1, 1].scatter(data1, data2, c=data3, cmap='RdBu_r')
fig.colorbar(pc, ax=axs[1, 1], extend='both')
axs[1, 1].set_title('scatter()')
```

### 使用多个Figure和Axes

```python
fig, axd = plt.subplot_mosaic([['upleft', 'right'],
                               ['lowleft', 'right']], layout='constrained')
axd['upleft'].set_title('upleft')
axd['lowleft'].set_title('lowleft')
axd['right'].set_title('right')
```
