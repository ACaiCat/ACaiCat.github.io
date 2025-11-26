---
title: "从0.1开始学C++和算法"
date: 2025-10-22T13:12:00+08:00
draft: false
tags: ["C++", "算法"]
---

## 起因

大概是 604 分考进了福专的给排水，所以考虑转专业了，但是大一转专业考高数干不过别人，所以决定大二靠机试转，然后顺便学学算法和 C++。

## 为什么是从 0.1？

因为本来就有一些其他语言基础，和一点点 C++的基础。

## C++基础

### 结构体

```cpp
#include <iostream>
#include <string>

using namespace std; //欸，懒得写命名空间了

struct Book // 定义结构体，和go是反的，哈哈哈哈
{
    int id;
    string name;
    string author;
};

int main()
{
    Book harryPotter = Book{1, "Harry James Potter", "J. K. Rowling"}; //初始化结构体
    harryPotter.id = 2; // 使用.访问结构体成员

    printf("ID: %d, Name: %s, Author: %s\n",
           harryPotter.id,
           harryPotter.name.c_str(),
           harryPotter.author.c_str());

    Book *lePetitPrince = new Book{2, "Le Petit Prince", "Antoine de Saint-Exupéry"}; //用new新建结构体指针，go直接对结构体取址就行了
    lePetitPrince->id = 1; //使用->访问结构体成员

    printf("ID: %d, Name: %s, Author: %s\n",
           lePetitPrince->id,
           lePetitPrince->name.c_str(),
           lePetitPrince->author.c_str());
}
```

### 指针

#### 指针基础

```cpp
#include <iostream>
#include <cstring>

using namespace std;

struct Book
{
    int id;
    string name;
};

int main()
{
    // 在堆上分配Book的内存，并且返回Book实例的指针
    auto book = new Book{1, "BookA"};

    // 使用->访问结构体指针的字段
    cout << book->id << "\n";

    // 必须手动释放，否则会造成内存泄漏
    delete book;

    size_t size = 10;

    // 在堆上分配int数组的内存，长度为size
    auto numbers = new int[size];

    // 使用memset分配统一初始值
    memset(numbers, 0, size * sizeof(int));

    // 遍历指针数组
    for (auto i = 0; i < size; i++)
    {
        cout << numbers[i] << "\n";
    }

    // 手动释放指针数组
    delete[] numbers;
}
```

#### 普通指针风险

- 内存泄漏

```cpp
void foo(){
    // 在堆上分配Book的内存，并且返回Book实例的指针
    auto book = new Book{1, "Book"};

    // 模拟运行时抛出异常
    throw runtime_error("A unususal error ~");

    // 抛出异常后分配在堆上的Book实例无法被清理，造成内存泄漏
    delete book;
}
```

- 野指针

```cpp
void foo(){
    auto book = new Book{1, "Book"};

    // 释放book内存，book变成野指针(悬空指针)
    delete book;

    // 访问野指针，发生神秘行为
    cout << book->id << "\n";
}
```

- 多次释放内存

```cpp
void foo(){
    auto book = new Book{1, "Book"};

    // 释放book内存
    delete book;

    // Booooooom!
    delete book;
}
```

#### 智能指针

规避上述指针风险的最好方法就是不要使用new/delete管理内存，智能指针就是一个很好的选择

```cpp
#include <iostream>
#include <memory>

using namespace std;

struct Book
{
    int id;
    string name;

    // 添加构造函数
    Book(int i, const string &n) : id(i), name(n) {}
};

int main()
{
    // 使用智能指针创建Book指针
    auto book = make_unique<Book>(1, "BookA");

    // 使用->访问结构体指针的字段
    cout << book->id << "\n";

    // 无需手动释放，自动管理

    size_t size = 10;

    // 使用智能指针创建int数组指针
    auto numbers = make_unique<int[]>(size);

    // 遍历指针数组初始化
    for (auto i = 0; i < size; i++)
    {
        numbers[i] = 0;
    }

    // 遍历指针数组
    for (auto i = 0; i < size; i++)
    {
        cout << numbers[i] << "\n";
    }

    // 无需手动释放，自动管理
}
```

#### 函数指针

```cpp
#include <iostream>
#include <cstring>

using namespace std;

int add(int x, int y)
{
    return x + y;
}

int subtract(int x, int y)
{
    return x - y;
}

int main()
{
    // 方法指针 返回类型 (*指针变量名)(参数,...)
    int (*operate)(int, int);

    int type = 1;
    switch (type)
    {
    case 1:
        operate = add;
        break;/
    case 2:
        operate = subtract;
        break;
    }
    cout << operate(114, 514) << "\n";
}
```

## 算法基础

### 枚举

枚举的思想是猜测和尝试，从可能的集合中注意尝试，判断题目条件成立，以此猜测答案。

#### 要点

1. 给出解空间
2. 减少枚举的空间
3. 选择合适的枚举顺序

#### 例题

- 一个数组中的数互不相同，求其中和为0的数对的个数。

```cpp
// Cai原版

#include <iostream>

using namespace std;

int main()
{
    int nums[] = {-1, -2, -3, 1, 2, 3, 5, 6};
    size_t length = sizeof(nums) / sizeof(int);

    int count = 0;

    for (int x = 0; x < length; x++)
    {
        // 发现有一半的过程是对称的，所以y < x，最后的count*2
        // 这样复杂度就下降了一半
        for (int y = 0; y < x; y++)
        {
            if (nums[x] + nums[y] == 0)
            {
                count++;
            }
        }
    }

    count *= 2;

    cout << count << "\n";
}
```  

```cpp
// 看完题解的优化版
#include <iostream>

using namespace std;

constexpr int MAXN = 10; // 桶最大容量

int main()
{
    int nums[] = {-1, -2, -3, 1, 2, 3, 5, 6};
    size_t length = sizeof(nums) / sizeof(int);

    // 创建一个桶，存储-MAXN~MAXN范围的数
    bool bucket[MAXN * 2 + 1] = {};
    int count = 0;

    for (int x = 0; x < length; x++)
    {
        int num = nums[x];
        // 检查是否有负的num在桶里，有的话条件成立直接+1
        if (bucket[MAXN - num])
        {
            count++;
        }
        // 不管结果如何，把数放进桶里
        bucket[MAXN + num] = true;
    }

    count *= 2;

    cout << count << "\n";
}
```

- [2811:熄灯问题](http://bailian.openjudge.cn/practice/2811/)
