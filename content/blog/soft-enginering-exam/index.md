---
title: "计算机学院转专业考试准备"
date: 2025-11-26T18:54:00+08:00
draft: false
tags: ["福州大学" ,"转专业" ,"C++", "Python", "算法"]
---

> Cai觉得有必要重温的题目.jpg  
> 解答均为Cai自己写的，不代表最优解

## PTA

1. [L1-002  打印沙漏](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805145370476544)
思路：先用模拟求一下需要使用的符号和层数，然后再输出即可

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        char c;
        cin >> n >> c;

        int req=0;
        int l=1;
        while (2*l*l-1<=n)
        {
            l++;
        }
        l--;
        req = 2*l*l-1;

        for (int i=l;i>=1;i--)
        {
            cout << string(l-i,' ')  << string(2*i-1,c) << '\n'; 
        }
        for (int i=2;i<=l;i++)
        {
            cout << string(l-i,' ') <<  string(2*i-1,c) << '\n'; 
        }
        cout << n-req;
    }
    ```

2. [L1-006 连续因子](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805138600869888)
思路：从2为开头，长度len寻找满足条件的因数列(n%因数积==0)，坑是遇到质数，因数是他本身

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        ll n;
        cin >> n;

        ll head=0,len=0;

        for (ll i=2;i*i<=n;i++)
        {
            for (ll l=len;;l++)
            {
                ll s=1;
                for (ll k=0;k<l;k++)
                {
                    s*=i+k;
                }
                if (s>n)
                {
                    break;
                }
                if (n%s==0&&l>len)
                {
                    head=i;
                    len=l;
                }
            }    
        }

        if (len==0)
        {
            len=1;
            head=n;
        }

        cout << len << '\n';

        string sep;
        for (ll k=0;k<len;k++)
        {
            cout << sep << head+k;
            sep="*";
        }

    }
    ```

3. [L1-009 N个数求和](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805133597065216)
思路：定义一个和的分子分母a、b，然后用公式去算，最后用__gcd()求最大公约数化简，坑是有一个测试点是0，要单独处理

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        cin >> n;
        
        ll a=0,b=1;
        for (int i=0;i<n;i++)
        {
            int x,y;
            scanf("%d/%d",&x,&y);
            a=a*y+x*b;
            b=b*y;
            // a   x   a*y+x*b
            // - + - = -------
        } 
        
        if (a==0)
        {
            cout << 0;
            return 0;
        }
        
        ll g = __gcd(a,b);
        a/=g;
        b/=g;
    
        if (a*b<0)
        {
            a=-abs(a);
            b=abs(b);
        }
        
        ll integer = a/b;
        a-=integer*b;
        
        string sep;
        if (integer!=0)
        {
            cout << integer;
            sep=" ";
        }
        
        if (a!=0)
        {
            printf("%s%d/%d",sep.c_str(),a,b);
        }
    
    }
    ```

4. [L1-020 帅到没朋友](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805117167976448)
思路：这题的难点在于理解题目，没有朋友的人指的是发朋友圈只有一个人，那么那个人就没朋友，或者查询的时候查无此人，也是没有朋友。所以只要用集合把所有朋友圈人数>1的人记下来，然后输出查询结果即可

    ```cpp
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        cin >> n;
        unordered_set<int> people;
        for (int i=0;i<n;i++)
        {
        int m;
        cin >> m;

        if (m==1)
        {
            int x;
            cin >> x;
            continue;
            }

        for (int j=0;j<m;j++)
        {
            int x;
            cin >> x;
            people.insert(x);
            }
        }

        int m;
        cin >> m;
        unordered_set<int> query;
        bool has_handsome=false;
        string sep;
        for (int i=0;i<m;i++)
        {
            int x;
            cin >> x;
            if (!query.count(x))
            {
                if (!people.count(x))
                {
                    printf("%s%05d",sep.c_str(),x);
                    sep=" ";
                    has_handsome=true;
                }
                query.insert(x);
            }
            
        }
        if (!has_handsome)
        {
            cout << "No one is handsome";
        }
    }
    ```

5. [L1-028 判断素数](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805106325700608)
思路：先特殊情况排除x<2不是质数，然后再判断x==2是质数，然后再排除偶数，最后循环用奇数验证x是否为质数，坑是数范围是2的31次方，刚好不能用int(2的31次方-1)，得用long long存(2的64次方-1)

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    bool is_prime(ll x)
    {
        if (x<2)
        {
            return false;
        }
        if (x==2)
        {
            return true;
        }
        if ((x&1)==0)
        {
            return false;
        }
        
        for (int i=3;i*i<=x;i+=2)
        {
            if (x%i==0)
            {
                return false;
            }
        }
        return true;
        
    }

    int main() 
    {
        int n;
        cin >> n;
        for (int i=0;i<n;i++)
        {
            ll x;
            cin >> x;
            if (is_prime(x))
            {
                cout << "Yes" << '\n'; 
            }
            else
            {
                cout << "No" << '\n';
            }
        }
    }
    ```

6. [L1-032 Left-pad](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805100684361728)
思路：先判断字符串长度和目标长度，不够就用`string`补齐空位，超出就用`substr`截取，坑就是如果你用`cin`读取前两个数据，那么你的`cin`的缓冲区会剩余一个换行符，你需要在`getline`读取字符串之前调用`cin.ignore()`忽略掉缓冲区的换行符。

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        char c;
        string s;
        cin >> n >> c;
        cin.ignore();
        getline(cin,s);
        
        if (n>=s.size())
        {
            s=string(n-s.size(),c) + s;
        }
        else
        {
            s=s.substr(s.size()-n,n);
        }
        
        cout << s;
    }
    ```

7. [L1-039 古风排版](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805091888906240)
思路：先把字符串存到分片存到列里面，当然可以用`substr`，我懒就直接遍历字符串了，然后`reverse`反转一下，最后在按照格式打印就行了，坑就是每列的字符数不够要补空格不然会吃WA

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        cin >> n;
        string s;
        cin.ignore();
        getline(cin,s);
        
        vector<string> column;
        
        string s2;
        for (int i=0;i<s.size();i++)
        {
            
            s2+=s[i];
            if (s2.size()==n)
            {
                column.push_back(s2);
                s2="";
            }
        }
        if (s2!="")
        {
            column.push_back(s2);
        }
        reverse(column.begin(),column.end());
        for (int r=0;r<n;r++)
        {
            for (int c=0;c<column.size();c++)
            {
                if (r>=column[c].size())
                {
                    cout << " ";
                }
                else
                {
                    cout << column[c][r];
                }
            }
            cout << '\n';
        }
    }
    ```

8. [L1-043 阅览室](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805087447138304)
思路：模拟，没啥难的。坑是输入可能有问题，所以结束的书要及时`erase`掉，还有`round(double,double)`返回值还是`double`要转为`int`不然格式化会出事233

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        cin >> n; 
        
        int day=0;
        unordered_map<int,int> books;
        int t=0,p=0;
        for (;;)
        {
            int id,h,m;
            char c;
            
            cin >> id >> c;
            scanf("%d:%d",&h,&m);
            if (id==0)
            {
                day++;
                if (p==0)
                {
                    printf("0 0\n");
                }
                else
                {
                    printf("%d %d\n",p,int(round(t*1.0/p)));
                }
                
                t=0;
                p=0;
                
                if (day==n)
                {
                    break;
                }
                continue;
            }
            
            if (c=='S')
            {
                books[id] = h*60+m;
            }
            else
            {
                if (books.count(id))
                {
                    p++;
                    t+=(h*60+m)-books[id];
                    books.erase(id);
                }
            }
        }
    }
    ```

9. [L1-046 整除光棍](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805084284633088)
思路：看数据范围应该是高精度，直接无脑选Python，注意Python的整除是`//`

    ```python
    x=int(input())
    n=1
    c=1
    while n%x!=0:
        n*=10
        n+=1
        c+=1

    print(n//x,c)
    ```

10. [L1-049 天梯赛座位分配](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805081289900032)
思路：给每个学校都准备个`vector`表示每个学校的座位，然后把这些`vector`打包塞进一个`vector`表示所有学校，然后主循环中维护变量`i`表示每个学校对应的座位索引，还有变量`seat`表示座位号，遍历学校`vector`，通过`i`给每个学校相同队伍位置赋值`seat`，每赋值一次`seat++`，直到只剩一个学校`seat+=2`，最后输出结果。坑没多少，但是大模拟很考验耐心，慢慢调试。

    ```cpp
    #include <iostream>
    #include <bits/stdc++.h>

    using namespace std;

    using ll = long long;

    int main() 
    {
        int n;
        cin >> n;
        vector<vector<int>> school;
        
        for (int i=0;i<n;i++)
        {
            int k;
            cin >> k;
            vector<int> seat(k*10);
            school.push_back(seat);
        }
        
        int remain_school=n;
        
        int seat=1;
        for (int i=0;remain_school>0;i++;)
        {
            for (auto &s:school)
            {
                if (i<s.size())
                {
                    s[i]=seat;
                    
                    if (remain_school==1)
                    {
                        seat+=2;
                    }
                    else
                    {
                        seat++;
                    }
                    
                    if (i==s.size()-1)
                    {
                        remain_school--;
                    }
                }
            }
        }
        
        int id=1;
        for (auto &s:school)
        {
            printf("#%d\n",id);
            string sep;
            for (int i=0;i<s.size();i++)
            {
                cout << sep << s[i];
                sep=" ";
                
                if (i%10==9)
                {
                    sep="";
                    cout << '\n';
                }
            }
            id++;
        }
    }
    ```

11. [L1-050](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805080346181632)
12. [L1-054](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805076512587776)
13. [L1-058](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599408664577)
14. [L1-064](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599412858885)
15. [L1-071](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1336215880692482054)
16. [L1-072](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1336215880692482055)
17. [L1-087](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1518581903422062592)
18. [L1-088](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1518582000729911296)
19. [L1-094](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1649748772841508869)
20. [L1-095](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1649748772841508870)
21. [L1-101](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1781658570803388420)
22. [L1-104](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1781658570803388423)
23. [L1-111](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1913922872972247046)
24. [L1-112](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1913922872972247047)
25. [L1-059](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599412858880)

## LuoGu

1. [P1803](https://www.luogu.com.cn/problem/P1803)
2. [P2240](https://www.luogu.com.cn/problem/P2240)
3. [P2241](https://www.luogu.com.cn/problem/P2241)
4. [P1217](https://www.luogu.com.cn/problem/P1217)
5. [P1036](https://www.luogu.com.cn/problem/P1036)
6. [P2615](https://www.luogu.com.cn/problem/P2615)
7. [P5732](https://www.luogu.com.cn/problem/P5732)
8. [P1205](https://www.luogu.com.cn/problem/P1205)
9. [P1042](https://www.luogu.com.cn/problem/P1042)
10. [P4924](https://www.luogu.com.cn/problem/P4924)
11. [P1045](https://www.luogu.com.cn/problem/P1045)
12. [P1249](https://www.luogu.com.cn/problem/P1249)
13. [P1012](https://www.luogu.com.cn/problem/P1012)
14. [P1923](https://www.luogu.com.cn/problem/P1923)
15. [P1116](https://www.luogu.com.cn/problem/P1116)
16. [P1068](https://www.luogu.com.cn/problem/P1068)
17. [P1706](https://www.luogu.com.cn/problem/P1706)
18. [P2249](https://www.luogu.com.cn/problem/P2249)

## 真题

1. [Lutece 167 a ^ b](https://cdoj.site/d/lutece/p/Lutece0167)
2. [最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)
3. [兔子试毒](https://www.luogu.com.cn/problem/T158663)
4. [淹没岛屿](https://www.luogu.com.cn/problem/P8662)
5. [质因数分解](https://judge.codemao.cn/problem/2256)
6. [螺旋举证](https://leetcode.cn/problems/spiral-matrix/description/)
7. [插队](https://www.luogu.com.cn/problem/U563768)
