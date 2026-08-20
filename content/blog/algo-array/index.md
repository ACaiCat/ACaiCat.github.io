---
title: "算法学习笔记：数组"
date: 2026-08-17T18:00:00+08:00
draft: true
tags: ["算法", "数组", "C++"]
---

## 二分查找

> 应用于**有序线性表**的高效查找算法

### [704. 二分查找](https://leetcode.cn/problems/binary-search/)

没学习前试了下直接枚举，复杂度是 O(n)，但是 LeetCode 不让这样玩，交上去被扫出来了

```cpp+
class Solution {
public:
    int search(vector<int>& nums, int target) {
        for (int i=0;i<nums.size();i++) {
            if (nums[i]==target) return i;
        }

        return -1;
    }
};
```

学了一下发现其实挺简单的，原理就是每次取中间的数和目标数比较，如果中间数大于目标数，那么目标数只可能在左边，反之亦然，然后一左一右不断缩小范围，直到找到目标数或者范围为空。

![二分查找示意图](binary-search.png)

AC 代码：

```cpp
class Solution
{
public:
    int search(vector<int> &nums, int target)
    {
        // 定义左右指针
        int left = 0;
        int right = nums.size() - 1;

        // 如果左指针小于等于右指针，说明还有搜索空间，大于则说明没有找到
        while (left <= right)
        {
            // 计算中间位置
            int middle = (right + left) / 2;

            // 中间大说明目标在左边，右指针左移，裁切右半部分
            if (nums[middle] > target)
            {
                right = middle - 1;
            }
            // 中间小说明目标在右边，左指针右移，裁切左半部分 
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            // 中间等于目标说明找到了，返回中间位置
            else if (nums[middle] == target)
            {
                return middle;
            }
        }

        // 没有找到目标数，返回 -1
        return -1;
    }
};
```

- 时间复杂度：O(log n)
- 空间复杂度：O(1)

> [!WARNING]
> 使用二分查找的前提是数组必须是有序的，如果数组无序，必须先排序再使用二分查找。  
> 如果数组中有重复元素，二分查找只能找到其中一个目标值的位置，不能保证找到所有目标值的位置。

### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

AC 代码：

```cpp
class Solution
{
public:
    int searchInsert(vector<int> &nums, int target)
    {
        // 前半部分就是正常二分查找
        int left = 0;
        int right = nums.size() - 1;
        int middle = 0;
        while (left <= right)
        {
            middle = (left + right) / 2;
            if (nums[middle] > target)
            {
                right = middle - 1;
            }
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            else
            {
                return middle;
            }
        }

        // 后半部分插入这里比较绕
        
        // 如果目标数小于中间数
        // 说明目标数应该插入到中间数的前面
        if (nums[middle] > target)
        {
            // 插入后，插入数会替代中间数的位置
            // 所以返回中间数的位置
            return middle;
        }
        // 如果目标数大于中间数
        // 说明目标数应该插入到中间数的后面
        else
        {
            // 插入数会插入到中间数的后面
            // 所以返回中间数的位置 + 1
            return middle + 1;
        }
    }
};
```

- 时间复杂度：O(log n)
- 空间复杂度：O(1)

另一种思路：

```cpp
class Solution
{
public:
    int searchInsert(vector<int> &nums, int target)
    {
        // 前面不变
        // 感觉更烧脑子了...
        ...
        
        // 分别处理如下三种情况
        // 目标值在数组所有元素之前  [0, -1]
        // 目标值插入数组中的位置 [left, right]，return  right + 1
        // 目标值在数组所有元素之后的情况 [left, right]， 因为是右闭区间，所以 return right + 1
        return right + 1;
    }
};
```

### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

没做出来，看的题解：

```cpp
// 时间复杂度：O(log n) + O(log n) = O (2 log n) = O(log n) [系数忽略]
class Solution
{
public:
    vector<int> searchRange(vector<int> &nums, int target)
    {
        // 开始位置和结束位置
        // -1, -1 代表没有找到
        int start = -1;
        int end = -1;
        
        // 第一个二分找左边界 start
        int left = 0;
        int right = nums.size() - 1;
        while (right >= left)
        {
            int middle = (right + left) / 2;
            if (nums[middle] > target)
            {
                right = middle - 1;
            }
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            else
            {
                // 找到后记下位置，向左继续找，找到左边界
                start = middle;
                right = middle - 1;
            }
        }

        // 第二个二分找右边界 end
        left = 0;
        right = nums.size() - 1;
        while (right >= left)
        {
            int middle = (right + left) / 2;
            if (nums[middle] > target)
            {
                right = middle - 1;
            }
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            else
            {
                // 找到后记下位置，向右继续找，找到右边界
                end = middle;
                left = middle + 1;
            }
        }

        return vector({start, end});
    }
};
```

### [69. x 的平方根](https://leetcode.cn/problems/sqrtx/)

一开始自己搓的大份：

```cpp
class Solution
{
public:
    int mySqrt(int x)
    {
        // 排除特殊情况
        if (x == 0)
        {
            return 0;
        }
        if (x == 1)
        {
            return 1;
        }
        long long left = 0;
        long long right = x;
        int result = -1;
        while (left <= right)
        {
            long long middle = (left + right) / 2;
            // 除 0 退出
            if (middle == 0)
            {
                break;
            }
            // 计算商，避免溢出
            long long quotient = x / middle;
            if (quotient * quotient < x)
            {
                // 商的平方小符合条件
                // 但还要继续找更大的商，右指针左移
                right = middle - 1;
                result = quotient;
            }
            else if (quotient * quotient > x)
            {
                // 商的平方大不符合条件，左指针右移
                left = middle + 1;
            }
            else
            {
                // 特殊情况，商的平方等于 x，直接返回商
                return quotient;
            }
        }
        return result;
    }
};
```

然后看了题解，稍微优化了一下：

```cpp
class Solution
{
public:
    int mySqrt(int x)
    {
        int left = 0;
        int right = x;
        int result = -1;
        while (left <= right)
        {
            // 直接用 long long 避免溢出
            long long middle = (long long)(left + right) / 2;
            if ((long long)middle * middle > x)
            {
                // middle 的平方大于 x，右指针左移
                right = middle - 1;
            }
            else
            {
                // middle 的平方小于等于 x，满足条件
                // 但还要继续找更大的平方根，左指针右移
                left = middle + 1;
                result = middle;
            }
        }
        return result;
    }
};
```

### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

本质和上一题一样，但是比上一题简单，只需要判断能不能被整的开平方

```cpp
class Solution
{
public:
    bool isPerfectSquare(int num)
    {
        int left = 0;
        int right = num;
        while (left <= right)
        {
            long long middle = (long long)(left + right) / 2;
            if ((long long)middle * middle > num)
            {
                right = middle - 1;
            }
            else if ((long long)middle * middle < num)
            {
                left = middle + 1;
            }
            else
            {
                return true;
            }
        }
        return false;
    }
};
```

## 移除元素

### [27. 移除元素](https://leetcode.cn/problems/remove-element/)

没学，自己随便试了一下，过了，草飞了`0.71%`的人：

```cpp
class Solution
{
public:
    int removeElement(vector<int> &nums, int val)
    {
        // 移除元素个数
        int k = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            if (nums[i] == val)
            {
                k++;
                // 用 -1 标记被移除的元素
                nums[i] = -1;
            }
        }

        for (int i = 0; i < nums.size(); i++)
        {
            // 如果当前元素是 -1，说明被移除了，需要把后面的元素往前移动
            if (nums[i] == -1)
            {
                // 查找下一个不为 -1 的元素，替换当前元素
                for (int j = i + 1; j < nums.size(); j++)
                {
                    if (nums[j] != -1)
                    {
                        nums[i] = nums[j];
                        nums[j] = -1;
                        break;
                    }
                }
            }
        }

        return nums.size() - k;
    }
};
```

看了题解的暴力算法，更加简洁：

```cpp
// 时间复杂度：O(n^2)
class Solution
{
public:
    int removeElement(vector<int> &nums, int val)
    {
        int size = nums.size();

        for (int i = 0; i < size; i++)
        {
            // 如果当前元素等于要移除的元素，就把后面的元素往前移动一位，并且覆盖当前元素
            if (nums[i] == val)
            {
                for (int j = i + 1; j < size; j++)
                {
                    nums[j - 1] = nums[j];
                }
                // 数组长度缩小
                size--;
                // 因为当前元素被后面的元素覆盖了，所以回退一位，继续检查当前位置
                i--;
            }
        }

        return size;
    }
};
```

学了点双指针：

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        // 慢指针，指向下一个要放置的元素位置
        int slowIndex = 0;
        // 快指针，遍历整个数组
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {
            // 当前元素不等于要移除的元素时，把它放到慢指针的位置，并且慢指针向前移动
            // 当前元素等于要移除的元素时，跳过它，快指针继续向前移动
            if (val != nums[fastIndex]) {
                // 把当前元素放到慢指针的位置
                nums[slowIndex] = nums[fastIndex];
                // 慢指针向前移动
                slowIndex++;
            }
            
        }
        // 慢指针的值就是新数组的长度
        return slowIndex;
    }
};
```

### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

感觉和上一题差不多，关键点是覆盖重复的数：

```cpp
class Solution
{
public:
    int removeDuplicates(vector<int> &nums)
    {
        int slowIndex = 0;
        // 记录上一个元素的值，初始化为一个不可能出现的值
        int last = -101;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++)
        {
            if (last != nums[fastIndex])
            {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
            last = nums[fastIndex];
        }
        return slowIndex;
    }
};
```

### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

自己搓的，感觉和上一题差不多：

```cpp
class Solution
{
public:
    void moveZeroes(vector<int> &nums)
    {
        // 依旧双指针
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++)
        {
            if (nums[fastIndex] != 0)
            {
                nums[slowIndex] = nums[fastIndex];
                // 俩位置不相等的时候，把慢指针位置的数置为 0
                // 如果没有这个条件过不了 case [1]
                if (fastIndex != slowIndex)
                {
                    nums[fastIndex] = 0;
                }
                slowIndex++;
            }
        }
    }
};
```

### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

简单用双指针做了一下，思路就是退格的时候慢指针回退一位，快指针继续往前走：

```cpp
class Solution
{
public:
    bool backspaceCompare(string s, string t)
    {
        // 记录最终字符串的长度
        int size = s.size();
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < s.size(); fastIndex++)
        {
            // 把当前字符放到慢指针的位置
            if (s[fastIndex] != '#')
            {
                
                s[slowIndex] = s[fastIndex];
                slowIndex++;
            }
            // 如果当前字符是退格符，慢指针回退一位，快指针继续往前走
            else
            {
                // 特殊情况，开头是退格符，慢指针不能回退
                // 只去掉退格符，size 减 1
                if (slowIndex == 0)
                {
                    size--;
                }
                // 慢指针回退一位，快指针继续往前走
                // 需要去掉字符和退格符，所以 size 减 2
                else
                {
                    slowIndex--;
                    size -= 2;
                }
            }
        }

        // 和上面一样
        int size2 = t.size();
        int slowIndex2 = 0;
        for (int fastIndex = 0; fastIndex < t.size(); fastIndex++)
        {
            if (t[fastIndex] != '#')
            {
                t[slowIndex2] = t[fastIndex];
                slowIndex2++;
            }
            else
            {
                if (slowIndex2 == 0)
                {
                    size2--;
                }
                else
                {
                    slowIndex2--;
                    size2 -= 2;
                }
            }
        }

        // 长度不相等直接返回 false
        if (size != size2)
        {
            return false;
        }

        // 逐个比较字符是否相等
        for (int i = 0; i < size; i++)
        {
            if (s[i] != t[i])
            {
                return false;
            }
        }

        return true;
    }
};
```

## 有序数组的平方

### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

想到是要用双指针然后按照绝对值大小排序，但是不会写喵，直接暴力了：

```cpp
// O (n log n) 的复杂度
class Solution
{
public:
    vector<int> sortedSquares(vector<int> &nums)
    {
        // O (n) 的复杂度
        for (int i = 0; i < nums.size(); i++)
        {
            nums[i] = nums[i] * nums[i];
        }
        // sort 是 O(n log n) 的复杂度
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```
