---
title: "从0.1开始学C++和算法"
date: 2025-10-22T13:12:00+08:00
draft: false
tags: ["C++", "算法"]
---

## 起因

大概是 603 分考进了福专的给排水，所以考虑转专业了，但是大一转专业考高数干不过别人，所以决定大二靠机试转，然后顺便学学算法和 C++。

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
