---
title: "计算机学院转专业考试准备"
date: 2025-11-26T18:54:00+08:00
draft: false
tags: ["福州大学" ,"转专业" ,"C++", "Python", "算法"]
---

> Cai觉得有必要重温的题目.jpg  
> 解答均为Cai自己写的，不代表最优解

## PTA

### 1. [L1-002  打印沙漏](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805145370476544)

思路：先用模拟求一下需要使用的符号和层数，然后再输出即可

```cpp
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

### 2. [L1-006 连续因子](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805138600869888)

思路：从`2`为开头，长度`len`寻找满足条件的因数列(`n%因数积==0`)，坑是遇到质数，因数是他本身

```cpp
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

### 3. [L1-009 N个数求和](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805133597065216)

思路：定义一个和的分子分母`a、b`，然后用公式去算，最后用`__gcd()`求最大公约数化简，坑是有一个测试点是0，要单独处理

```cpp
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
        // b   y     b*y
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

### 4. [L1-020 帅到没朋友](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805117167976448)

思路：这题的难点在于理解题目，没有朋友的人指的是发朋友圈只有一个人，那么那个人就没朋友，或者查询的时候查无此人，也是没有朋友。所以只要用集合把所有朋友圈人数`>1`的人记下来，然后输出查询结果即可

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

### 5. [L1-028 判断素数](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805106325700608)

思路：先特殊情况排除`x<2`不是质数，然后再判断`x==2`是质数，然后再排除偶数，最后循环用奇数验证`x`是否为质数，坑是数范围是2的31次方，刚好不能用`int`($2^{31}-1$)，得用`long long`存($2^{63}-1$)

```cpp
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

### 6. [L1-032 Left-pad](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805100684361728)

思路：先判断字符串长度和目标长度，不够就用`string`补齐空位，超出就用`substr`截取，坑就是如果你用`cin`读取前两个数据，那么你的`cin`的缓冲区会剩余一个换行符，你需要在`getline`读取字符串之前调用`cin.ignore()`忽略掉缓冲区的换行符。

```cpp
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

### 7. [L1-039 古风排版](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805091888906240)

思路：先把字符串存到分片存到列里面，当然可以用`substr`，我懒就直接遍历字符串了，然后`reverse`反转一下，最后在按照格式打印就行了，坑就是每列的字符数不够要补空格不然会吃WA

```cpp
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

### 8. [L1-043 阅览室](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805087447138304)

思路：模拟，没啥难的。坑是输入可能有问题，所以结束的书要及时`erase`掉，还有`round(double,double)`返回值还是`double`要转为`int`不然格式化会出事233

```cpp
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

### 9. [L1-046 整除光棍](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805084284633088)

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

### 10. [L1-049 天梯赛座位分配](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805081289900032)

思路：给每个学校都准备个`vector`表示每个学校的座位，然后把这些`vector`打包塞进一个`vector`表示所有学校，然后主循环中维护变量`i`表示每个学校对应的座位索引，还有变量`seat`表示座位号，遍历学校`vector`，通过`i`给每个学校相同队伍位置赋值`seat`，每赋值一次`seat++`，直到只剩一个学校`seat+=2`，最后输出结果。坑没多少，但是大模拟很考验耐心，慢慢调试。

```cpp
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

### 11. [L1-050 倒数第N个字符串 (⭐155学长推荐)](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805080346181632)

思路：使用26进制做，类比10机制，`a=0`，`z=25`，每26进1，也就是`z+1=aa`。首先用`for`循环构建一个有l个z的26进制数`x`，然后减去`n-1`(因为是倒数第n个数，所以要减一，因为倒数第一就是x)，然后使用`do-while (x>0)`把结果拆开转为字符存进字符串，这样子构建的字符串是倒着的，你可以倒序输出也可以像我一样直接用`reverse`反转，最后记得要用`a`补位，保证输出长度也是`l`

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{
    ll l,n;
    cin >> l >> n;
    
    ll x=0;
    for (int i=0;i<l;i++)
    {
        x*=26;
        x+=25;
    }
    
    x-=n-1;
    string s;
    do
    {
        s+=char(x%26+'a');
        x/=26;
    } while (x>0);
    
    reverse(s.begin(),s.end());
    
    cout << string(l-s.size(),'a')+s;
}
```

### 12. [L1-054 福到了](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=994805076512587776)

思路：创建一个`vector<vector<bool>>`记录有字符的格子，注意要读取的格子包含空格所以必须用`getchar`不能用`cin`，因为`cin`会跳过所有空白符，因为`getchar`会读取`\n`所以我们用一个`do-while`循环来跳过`\n`，然后我们用`new[i][j]=old[n-1-i][n-i-j]`来180度翻转二维容器，因为`vector`有`==`的运算符重载，所以我们可以直接用`==`比较翻转前后两个`vector`是否相等，最后输出即可。

| 变换类型 | 公式 | 示例 (3×3) |
|---------|------|------------|
| **顺时针90°** | `new[i][j] = old[n-1-j][i]` | `1 2 3` → `7 4 1`<br>`4 5 6` → `8 5 2`<br>`7 8 9` → `9 6 3` |
| **逆时针90°** | `new[i][j] = old[j][n-1-i]` | `1 2 3` → `3 6 9`<br>`4 5 6` → `2 5 8`<br>`7 8 9` → `1 4 7` |
| **180°旋转** | `new[i][j] = old[n-1-i][n-1-j]` | `1 2 3` → `9 8 7`<br>`4 5 6` → `6 5 4`<br>`7 8 9` → `3 2 1` |
| **水平镜像** | `new[i][j] = old[i][n-1-j]` | `1 2 3` → `3 2 1`<br>`4 5 6` → `6 5 4`<br>`7 8 9` → `9 8 7` |
| **垂直镜像** | `new[i][j] = old[n-1-i][j]` | `1 2 3` → `7 8 9`<br>`4 5 6` → `4 5 6`<br>`7 8 9` → `1 2 3` |  

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{
    char c;
    int n;
    cin >> c >> n;
    
    vector<vector<bool>> g(n,vector<bool>(n));
    
    for (auto &row:g)
    {
        for (auto &&col:row)
        {
            char x;
            do
            {
                x=getchar();
            } while (x=='\n');
            
            col = x=='@';
        }
    }
    
    vector<vector<bool>> d(n,vector<bool>(n));
    
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            d[i][j]=g[n-1-i][n-1-j];
        }
    }
    
    if (g==d)
    {
        cout << "bu yong dao le" << '\n';
    }
    
    for (auto &row:d)
    {
        for (auto &&col:row)
        {
            cout << (col?c:' ');
        }
        cout << '\n';
    }
}
```

### 13. [L1-058 6翻了](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599408664577)

思路：用正则表达式替换掉9个以上的连续`6`为`27`，然后替换掉3个以上的连续`6`为`9`，注意替换顺序

```python
import re
s=input()

s=re.sub("6{10,}","27",s)
s=re.sub("6{4,9}","9",s)

print(s)
```

### 14. [L1-064 估值一亿的AI核心代码](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599412858885)

思路：字符串处理首先想`Python`用正则表达式，不然会被恶心死，当然这题就算你用正则表达式也会被恶心。首先要先把字符串转为小写，这里按照题目要求排除`I`，然后处理空格，利用正则表达式删除所有不符合要求的空格，然后把2个以上的空格替换为1个空格，然后匹配`I`和`me`换位`__you`，注意不能直接换成`you`，可能会和下面的`can you`冲突，然后匹配完`can you`再把`__you`换回去

| 正则表达式 | 替换为 | 作用描述 |
|------------|--------|----------|
| `r"^ +"` | `""` | 删除字符串开头的所有空格 |
| `r" +$"` | `""` | 删除字符串末尾的所有空格 |
| `r" +(?=[!?.:'])"` | `""` | 删除标点符号(`!?.:'`)前的所有空格 |
| `r" {2,}"` | `" "` | 将两个或更多连续空格替换为单个空格 |
| `r"\b(I\|me)\b"` | `"__you"` | 将单词"I"或"me"临时替换为"__you" |
| `r"\bcan you\b"` | `"I can"` | 将"can you"转换为"I can" |
| `r"\bcould you\b"` | `"I could"` | 将"could you"转换为"I could" |
| `"__you"` | `"you"` | 将临时标记"__you"恢复为"you" |

```python
import re

n=int(input())
for _ in range(n):
    s=input()
    print(s)

    s=s.replace("?","!")
    s = "".join([ i.lower() if i!='I' else i for i in s])
    
    s=re.sub(r"^ +| +$| +(?=[!?.:'])","",s)
    s=re.sub(r" {2,}"," ",s)

    s=re.sub(r"\b(I|me)\b","__you",s)
    s=re.sub(r"\bcan you\b","I can",s)
    s=re.sub(r"\bcould you\b","I could",s)
    s=s.replace("__you","you")
    
    print("AI:",s)
```

### 15. [L1-071 前世档案](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1336215880692482054)

思路：发现每回答一次n，结论增加$2^{层数-1}$，所以就很简单了

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{
    int n,m;
    cin >> n >> m;
    for (int i=0;i<m;i++)
    {
        int ans=1;
        for (int j=n-1;j>=0;j--)
        {
            char c;
            cin >> c;
            if (c=='n')
            {
                ans+=pow(2,j);
            }
        }
        cout << ans << '\n';
    }
}
```

### 16. [L1-072 刮刮彩票](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1336215880692482055)

思路：又是一题超级烦人大模拟，首先应该先读取初始棋盘，但是有个坑，就是有一个`0`需要你填出来，可以先记一下`0`的位置，然后求一下和，最后用`45-和`就是`0`的点数，还有个坑就是求列的点数和的时候得`d-3-1`，最后建个`unordered_map`映射点数和金币(傻逼)

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

unordered_map<int,int> coins = 
{
 {6,10000},{7,36},{8,720},{9,360},{10,80},{11,252},{12,108},
 {13,72},{14,54},{15,180},{16,72},{17,180},{18,119},{19,36},
 {20,306},{21,1080},{22,114},{23,1800},{24,3600}
};

int main() 
{
    int g[3][3];
    
    int init_x,init_y,sum=0;
    for (int r=0;r<3;r++)
    {
        for (int c=0;c<3;c++)
        {
            int x;
            cin >> x;
            g[r][c]=x;
            sum+=x;
            if (x==0)
            {
                init_x=r;
                init_y=c;
            }
        }
    }
    
    g[init_x][init_y]=45-sum;
    
    for (int i=0;i<3;i++)
    {
        int x,y;
        cin >> x >> y;
        x--; y--;
        
        cout << g[x][y] << '\n';
    }
    
    int d,p=0;
    cin >> d;
    
    if (d<=3)
    {
        for (int c=0;c<3;c++)
        {
            p+=g[d-1][c];
        }
    }
    else if (d<=6)
    {
        for (int r=0;r<3;r++)
        {
            p+=g[r][d-1-3];
        }
    }
    else if (d==7)
    {
        for (int i=0;i<3;i++)
        {
            p+=g[i][i];
        }
    }
    else
    {
        for (int i=0;i<3;i++)
        {
            p+=g[i][3-1-i];
        }
    }
    cout << coins[p];
    
}
```

### 17. [L1-087 机工士姆斯塔迪奥](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1518581903422062592)

思路：首先创建图格`vector<vector<bool>>`存储地图，然后读取输入来将被炸的格子设为不安全，最后遍历整个地图，统计所有安全的图格

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{
    int n,m,q;
    cin >> n >> m >> q;
    vector<vector<bool>> g(n,vector<bool>(m));
    
    for (int i=0;i<q;i++)
    {
        bool column;
        int p;
        cin >> column >> p;
        p--;
        if (column)
        {
            for (int j=0;j<n;j++)
            {
                g[j][p]=true;
            }
        }
        else
        {
            for (int j=0;j<m;j++)
            {
                g[p][j]=true;
            }
        }
    }
    
    int res=0; 
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<m;j++)
        {
            if (!g[i][j])
            {
                res++;  
            }
        }
    }
    cout << res;
}
```

### 18. [L1-088 静静的推荐](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1518582000729911296)

思路：用`map<int,pair<int,int>>`存储天梯赛分数和PTA分数，其中PTA达标的和没达标的分开存到`pair<int,int>`中，输入时忽略天梯赛分数低于175分的学生(~~斩杀线~~)，然后遍历整个`map`，其中PTA分数达标的学生无论K够不够都可以直接被录取，所以直接`res+=p.second.first;`，PTA分数没达标的学生只能在K论中按照成绩被录取，所以去k和人数的最小值即`res+=min(k,p.second.second);`，注意这题使用模拟会直接超时，坑死了

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    int n,k,s;
    cin >> n >> k >> s;
    map<int,pair<int,int>> stu;
    
    for (int i=0;i<n;i++)
    {
        int gplt,pta;
        cin >> gplt >> pta;
        
        if (gplt<175)
        {
            continue;
        }
        
        if (pta>=s)
        {
            stu[gplt].first++;
        }
        else
        {
            stu[gplt].second++;
        }
    }
    int res=0;
    for (auto &p:stu)
    {
        res+=min(k,p.second.second);
        res+=p.second.first;
    }
    cout << res;   
}
```

### 19. [L1-094 剪切粘贴](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1649748772841508869)

思路：烦人的字符串模拟题，先把输入坐标转化为数组下标，`开始位置-1`、`结束位置`不变，不然处理起来会乱掉，然后就是按照要求去截取字符串，然后插入到对应位置，注意这里不要先找插入位置的前`s1`或后`s2`，应该把插入位置前后相加`s1+s2`再找，然后得到插入位置前的起始下标`p(s1)`，然后把`p+len(s1)`就是我们`cut`插入的起始下标，最后合并即可`s=s[:p+len(s1)] + cut + s[p+len(s1):]`，这里推荐用题目给的`abfg`调试，测试用例的数据太长了，可以最后验证，但是别拿来调试

```python
s=input()

n=int(input())
for _ in range(n):
    x,y,s1,s2=input().split()
    x=int(x)-1
    y=int(y)
    cut=s[x:y]
    s=s[:x]+s[y:]
    p=s.find(s1+s2)
    if p==-1:
        s+=cut
    else:
        s=s[:p+len(s1)] + cut + s[p+len(s1):]

print(s)
```

### 20. [L1-095 分寝室](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1649748772841508870)

思路：又是模拟模拟模拟，所以就按照题目要求模拟就行了，建一个`for`循环`i0`表示女生宿舍人数，`i1=n-i0`表示男生宿舍，按照题目要求筛选，复杂度是`O(n)`不算太慢可以接受，但是要注意条件，`i1`和`i0`是宿舍数，宿舍人数要另外算，别搞错了

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    int n0,n1,n;
    cin >> n0 >> n1 >> n;
    int min_abs=INT_MAX,min_i0=0;
    
    for (int i0=1;i0<n;i0++)
    {
        if (n0%i0!=0)
        {
            continue;
        }
        
        if (n0==i0)
        {
            continue;
        }
        
        int i1=n-i0;
        
        if (n1%i1!=0)
        {
            continue;
        }
        
        if (n1==i1)
        {
            continue;
        }
        
        int abs_diff=abs(n1/i1-n0/i0);
        if (abs_diff<min_abs)
        {
            min_abs=abs_diff;
            min_i0=i0;
        }
    }
    
    if (min_abs==INT_MAX)
    {
        cout << "No Solution";
    }
    else
    {
        cout << min_i0 << " " << n-min_i0;
    }
}
```

### 21. [L1-101 别再来这么多猫娘了！](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1781658570803388420)

思路：遍历违禁词，然后用`count`统计该违禁词出现次数，然后用`replace`替换成`____`防止违禁词冲突，最后把`____`换回`<censored>`

```python
n=int(input())

baned=[]
for _ in range(n):
    baned.append(input())

k=int(input())
s=input()

c=0
for w in baned:
    c+=s.count(w)
    s=s.replace(w,"____")

if c>=k:
    print(c)
    print("He Xie Ni Quan Jia!")
else:
    print(s.replace("____","<censored>"))
```

### 22. [L1-104 九宫格](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1781658570803388423)

思路：先检查输入数字是不是`1-9`(超坑)，然后用两层嵌套`for`循环分别检查每行每列的是否都有`1-9`，方法是建一个`unordered_set`然后检查`size()==9`，最后把大九宫格分成`9`个套`4`层`for`循环检查，坑就是检查的位置要对，要在结束`9`个数字遍历的时候检查

```cpp
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{ 
    int n;
    cin >> n;
    while (n--)
    {
        bool ok=true;
        vector<vector<int>> g(9,vector<int>(9));
        for (int r=0;r<9;r++)
        {
            for (int c=0;c<9;c++)
            {
                int x;
                cin >> x;
                g[r][c]=x;
                if (x>9||x<1)
                {
                    ok=false;
                }
            }
        }
        
        if (!ok)
        {
            cout << 0 << '\n';
            continue;
        }
        
        for (int i=0;i<9;i++)
        {
            unordered_set<int> s1,s2;
            for (int j=0;j<9;j++)
            {
                s1.insert(g[i][j]); 
                s2.insert(g[j][i]); 
            }
            if (s1.size()!=9||s2.size()!=9)
            {
                ok=false;
                break;
            }
        }
        
        if (!ok)
        {
            cout << 0 << '\n';
            continue;
        }
        
        for (int i=0;i<3;i++)
        {
            unordered_set<int> s;
            for (int j=0;j<3;j++)
            {
                for (int a=0;a<3;a++)
                {
                    for (int b=0;b<3;b++)
                    {
                        s.insert(g[i*3+a][j*3+b]); 
                    }
                }
                if (s.size()!=9)
                {
                    ok=false;
                    break;
                } 
            }
        }
        cout << ok << '\n';
    }
}
```

### 23. [L1-111 大幂数](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1913922872972247046)

思路：高精度问题首选`Python`，然后从次数100次方递减寻找，用一个`while`循环来增加连续数列长度，找到满足条件的直接输出然后`exit`退出即可，如果循环结束还是没找到直接默认没有 (非君子做法)

```python
n=int(input())

for k in range(100,0,-1):
    x=0
    l=0
    while x<n:
        l+=1
        x+=l**k

    if x==n:
        print("+".join([ f"{i}^{k}" for i in range(1,l+1) ]))
        exit()

print(f"Impossible for {n}.")
```

### 24. [L1-112 现代战争](https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1913922872972247047)

思路：这题时间限制不严，所以直接每次轰炸都遍历找一下最大价值的建筑，然后把所在行列全部`erase`掉，别忘了`n--`和`m--`

```cpp
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

int main() 
{ 
    int n,m,k;
    cin >> n >> m >> k;
    vector<vector<int>> g(n,vector<int>(m));
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<m;j++)
        {
            cin >> g[i][j];    
        }    
    } 
    
    while (k--)
    {
        int x,y,max_v=-1;
        for (int i=0;i<n;i++)
        {
            for (int j=0;j<m;j++)
            {
                if (g[i][j]>max_v)
                {
                    max_v=g[i][j];
                    x=i;
                    y=j;
                }
            }    
        }
        n--;
        m--;
        g.erase(g.begin()+x);
        
        for (int i=0;i<n;i++)
        {
            g[i].erase(g[i].begin()+y);
        }
    }
    
    for (int i=0;i<n;i++)
    {
        string sep;
        for (int j=0;j<m;j++)
        {
            cout << sep <<  g[i][j];
            sep=" ";
        }
        cout << '\n'; 
    } 
}
```

### 25. [L1-059 敲笨钟](<https://pintia.cn/problem-sets/994805046380707840/exam/problems/type/7?problemSetProblemId=1111914599412858880>)

思路：Python正则表达式爽题，直接用正则表达式匹配替换诗句就好了,发现有点挺莫名其妙的，有个测试用例的上半句只有一个`ong`，所以要用`^.*ong,.+ong\.$`的`.*`来匹配，挺奇葩的，我觉得是测试用例过于极端了

```python
import re

n=int(input())

for _ in range(n):
    s=input()
    if re.match(r"^.*ong,.+ong\.$",s) is None:
        print("Skipped")
    else:
        s = re.sub(r"(\w+ \w+ \w+)\.$", "qiao ben zhong.", s)
        print(s)
```

## LuoGu

### 1. [P1803 凌乱的yyy / 线段覆盖](https://www.luogu.com.cn/problem/P1803)

思路：线段覆盖问题的贪心就是让每条线的右端点尽量向小，把右端点小的排前面，然后依次摆放

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

struct Time
{
    int start;
    int end;
};

int main() 
{
    int n;
    cin >> n;
    vector<Time> g(n);
    for (auto& i:g)
    {
        cin >> i.start >> i.end;
    }
    sort(g.begin(),g.end(),[](Time a,Time b)
    {
        return a.end<b.end;
    });
    int res=0;
    int t=-1;
    for (auto& i:g)
    {
        if (t<=i.start)
        {
            res++;
            t=i.end;
        }
    }
    cout << res;
}
```

### 2. [P2240 部分背包问题](https://www.luogu.com.cn/problem/P2240)

思路：物品可分割的背包问题就是贪心，先把算出每种金币的单价`v/w`，然后用`sort`排序，最后依次塞进背包，可以全塞就全塞，不能全塞就`单价x剩余背包空间`，然后直接`break`输出答案

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

struct Coin
{
    int w;
    int v; 
    double price;
};

int main() 
{
    int n,t;
    cin >> n >> t;
    vector<Coin> g(n);
    for (auto& i:g)
    {
        int m,v;
        cin >> m >> v;
        i.w=m;
        i.v=v;
        i.price=v*1.0/m;
    }
    sort(g.begin(),g.end(),[](Coin a,Coin b)
    {
        return a.price>b.price;
    });
    double res=0;
    
    for (auto &i:g) 
    {
        if (t>i.w)
        {
            t-=i.w;
            res+=i.v;    
        }
        else
        {
            res+=t*i.price;
            break;
        }
    }
    printf("%.2lf",res); 
}
```

### 3. [P2241 统计方形（数据加强版）](https://www.luogu.com.cn/problem/P2241)

思路：分别枚举边长即可，然后算出该边长下的长方形个数`(n-长+1)*(m-宽+1)`，注意长方形个数要减去正方形个数，而且统计时需要使用`long long`否则会溢出

```cpp
#include <iostream>
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{   
 ll n,m;
 cin >> n >> m;
 
 ll rec = 0;
 ll sq=0;
 for (ll x=1;x<=n;x++)
 {
  for (ll y=1;y<=m;y++)
  {
   if (x==y)
   {
    sq+=(n+1-x)*(m+1-y);
   } 
   
   rec += (n+1-x)*(m+1-y);
     
  }
 }
 
 cout << sq << " " << rec-sq;
}
```

### 4. [P1217 回文质数](https://www.luogu.com.cn/problem/P1217)

思路：判断质数前面有了，判断回文数可以构建一个反转数，然后判断反转数是否和回文数相等，但是这俩还是会超时，所以可以排除偶数，遍历时把步长设为`2`

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

bool is_prime(int x)
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

bool is_pal(int x)
{
    int num=x;
    int reverse_num=0;
    do
    {
        reverse_num*=10;
        reverse_num+=x%10;
        x/=10;
    } while (x>0);
    if (num!=reverse_num)
    {
        return false;
    }
    return true;
} 

int main() 
{
    int a,b;
    cin >> a >> b;
    if (a%2==0)
    {
        a++;    
    } 
    for (int i=a;i<=b;i+=2) 
    {
        if (is_pal(i)&&is_prime(i))
        {
            cout << i << '\n';
        } 
    }
}
```

### 5. [P1036 选数](https://www.luogu.com.cn/problem/P1036)

思路：用`dfs`遍历所有情况，使用`start`来保证组合的有序性(不会产生[1,2]和[2,1]这样的重复)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

bool is_prime(int x)
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

vector<int> g;
int n,k;
int res=0;

void dfs(vector<int> nums,int start)  
{
    if (nums.size()==k)
    {
        int sum=accumulate(nums.begin(),nums.end(),0);
        if (is_prime(sum))
        {
            res++;
        }
        return;
    }
    
    for (int i=start;i<n;i++)
    {
        nums.push_back(g[i]);
        dfs(nums,i+1);
        nums.pop_back();
    }
    
}

int main() 
{
    cin >> n >> k;
    g.resize(n);
    for (auto& i:g)
    {
        cin >> i;    
    }
    vector<int> nums;
    dfs(nums,0);
    cout << res;
    
}
```

### 6. [P2615 神奇的幻方](https://www.luogu.com.cn/problem/P2615)

思路：按照要求完成4条规则即可，没啥好说的

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;


int main() 
{
    int n;
    cin >> n;
    vector<vector<int>> g(n,vector<int>(n));
    
    int x=0,y=n/2;
    g[x][y]=1;
    
    for (int i=2;i<=n*n;i++)
    {
        if (x==0&&y!=n-1)
        {
            x=n-1;
            y++;
            g[x][y]=i;
            continue;
        }
        if (x!=0&&y==n-1)
        {
            x--;
            y=0;
            g[x][y]=i;
            continue;
        }
        if (x==0&&y==n-1)
        {
            x++;
            g[x][y]=i;
            continue;
        }
        if (x!=0&&y!=n-1)
        {
            if (g[x-1][y+1]==0)
            {
                x--;
                y++;
            }
            else
            {
                x++;
            }
            g[x][y]=i;
            continue;
        }
    }
    
    for (int i=0;i<n;i++)
    {
        string sep;
        for (int j=0;j<n;j++)
        {
            cout << sep << g[i][j];
            sep=" ";
        }
        cout << '\n';
    }
}
```

### 7. [P5732 杨辉三角](https://www.luogu.com.cn/problem/P5732)

思路：首先先把第一行填充了，然后接下来每行行首行尾填充1，中间的填充正上方和左上方之和

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;


int main() 
{
    int n;
    cin >> n;
    vector<vector<int>> g;
    g.push_back({1});
    
    for (int i=1;i<n;i++)
    {
        vector<int> lz;
        lz.push_back(1);
        for (int j=1;j<i;j++)
        {
            int a=g[i-1][j];
            int b=g[i-1][j-1]; 
            
            lz.push_back(a+b);
        }
        lz.push_back(1);
        g.push_back(lz); 
    }
    
    for (int i=0;i<n;i++)
    {
        string sep;
        for (int j=0;j<=i;j++)
        {
            cout << sep << g[i][j];
            sep=" ";
        }
        cout << '\n';
    }
}
```

### 8. [P1205 方块转换](https://www.luogu.com.cn/problem/P1205)

思路：使用前面的举证变换的公式就能搞定了，记得要按照给的顺序，不可以乱调

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int n;

int turn(vector<vector<char>>& rg,vector<vector<char>> &tg)
{
    vector<vector<char>> temp(n,vector<char>(n));
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            temp[i][j]=rg[n-1-j][i];
        }
    }
    if (temp==tg)
    {
        return 1;
    }
    
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            temp[i][j]=rg[n-1-i][n-1-j];
        }
    }
    if (temp==tg)
    {
        return 2;
    }
    
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            temp[i][j]=rg[j][n-1-i];
        }
    }
    if (temp==tg)
    {
        return 3;
    }
    
    return 0;
}


int main() 
{
    cin >> n;
    vector<vector<char>> rg(n,vector<char>(n));
    
    for (auto &r:rg)
    {
        for (auto &c:r)
        {
            cin >> c;
        }
    }
    
    vector<vector<char>> tg(n,vector<char>(n));

    
    for (auto &r:tg)
    {
        for (auto &c:r)
        {
            cin >> c;
        }
    }
    
    vector<vector<char>> temp(n,vector<char>(n));
    
    int t=turn(rg,tg);
    if (t!=0)
    {
        cout << t;
        return 0;
    }
    
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            temp[i][j]=rg[i][n-1-j];
        }
    }
    if (temp==tg)
    {
        cout << 4;
        return 0;
    }
    
    t=turn(temp,tg);
    if (t!=0)
    {
        cout << 5;
        return 0;
    }
    
    if (rg==tg)
    {
        cout << 6;
        return 0;
    }
    
    cout << 7;
     
}
```

### 9. [P1042 乒乓球](https://www.luogu.com.cn/problem/P1042)

思路：把比赛胜负录进字符串中，因为可能有奇奇怪怪的东西所以需要排除掉，然后模拟两种比赛下的比分即可，注意两人分差要大于等于`2`比赛才算结束，并且比赛结束马上开始下一场，也就是说刚刚好结束也要输出`0:0`

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    string s;
    for (;;) 
    {
        char c;
        cin >> c;
        if (c=='E') break;
        if (c=='W' || c=='L') s+=c;
    }
    int w=0,l=0;
    for (char c:s)
    {
        if (c=='W') w++;
        if (c=='L') l++;
        
        if ((w>=11||l>=11)&&(abs(w-l)>=2))
        {
            printf("%d:%d\n",w,l);
            w=0;
            l=0;
        }
    }
    printf("%d:%d\n",w,l);
    
    cout << '\n';
    
    w=0,l=0;
    for (char c:s)
    {
        if (c=='W') w++;
        if (c=='L') l++;
        
        if ((w>=21||l>=21)&&(abs(w-l)>=2))
        {
            printf("%d:%d\n",w,l);
            w=0;
            l=0;
        }
    }
    printf("%d:%d\n",w,l);
    
}
```

### 10. [P4924 魔法少女小Scarlet](https://www.luogu.com.cn/problem/P4924)

思路：每次旋转把需要旋转的部分截取的`temp`中方便操作，按照旋转公式旋转后再赋回二维数组中，注意该用变量就用变量，不要搞太乱了

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

void magic(vector<vector<int>>& g,int x,int y,int r,int z)
{
    int n=g.size();
    int m=2*r+1;
    int sx=x-r;
    int sy=y-r;
    vector<vector<int>> temp(m,vector<int>(m));
    if (z==0)
    {
        for (int i=0;i<m;i++)
        {
            for (int j=0;j<m;j++)
            {
                temp[i][j]=g[sx+m-1-j][sy+i];
            }
        }
    }
    if (z==1)
    {
        for (int i=0;i<m;i++)
        {
            for (int j=0;j<m;j++)
            {
                temp[i][j]=g[sx+j][sy+m-1-i];
            }
        }
    }
    for (int i=0;i<m;i++)
    {
        for (int j=0;j<m;j++)
        {
            g[sx+i][sy+j]=temp[i][j];
        }
    }
    
}

int main() 
{
    int n,m;
    cin >> n >> m;
    vector<vector<int>> g(n,vector<int>(n));
    int k=1;
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n;j++)
        {
            g[i][j]=k;
            k++;
        }
    }
    
    for (int i=0;i<m;i++)
    {
        int x,y,r,z;
        cin >> x >> y >> r >> z;
        x--;
        y--;
        magic(g,x,y,r,z);
    }
    
    for (int i=0;i<n;i++)
    {
        string sep;
        for (int j=0;j<n;j++)
        {
            cout << sep << g[i][j];
            sep=" ";
        }
        cout << '\n';
    }
}
```

### 11. [P1045 麦森数](https://www.luogu.com.cn/problem/P1045)

思路：高精选`Python`，因为这个太大了所以得用快速幂`pow(2,p,500)`，然后求位数就变成问题了，我们求位数可以用公式$\left \lfloor \log_{10}{(2^{p}-1)} \right \rfloor -1 =  \left \lfloor \ p\times log_{10}{2} \right \rfloor -1$，这样位数就求出来了，然后就是`pow(2,p,500)-1`，然后用`str`转为字符串，用`.rjust(500,"0")`来右对齐补位，最后按照要求切割输出就行了

```python
import math

p=int(input())

print(math.floor(p*math.log10(2))+1)

res = str(pow(2,p,10**500)-1).rjust(500,"0")

for i in range(0,500+1,50):
    print(res[i:i+50])
```

### 12. [P1249 最大乘积](https://www.luogu.com.cn/problem/P1249)

思路：高精度用`Python`，要想乘积大那么就要让数字均匀从`2+3+4+5+...`，然后剩余值再从后向前匀，有个特殊情况就是剩余的数对于最后的数，那么我们就要给最后的数`+2`，剩下的数还是`+1`，比如：`2+3->8`余`3`但是只有两个数，所以我们必须把后面的`3+2`，然后`2+1`，最后得到`3+5=8`。还有就是`2+3=5`所以`4`以内直接输出本身即可

```python
import math

n = int(input())

if n <= 4:
    print(n)
    print(n)
    exit()

parts = []
current = 2
remaining = n

while remaining >= current:
    parts.append(current)
    remaining -= current
    current += 1

if remaining > 0:
    if remaining == parts[-1]:
        parts[-1] += 1
        remaining -= 1

    for i in range(len(parts)-1, -1, -1):
        if remaining <= 0:
            break
        parts[i] += 1
        remaining -= 1


product = 1
for num in parts:
    product *= num
    

print(" ".join(map(str, parts)))
print(product)
```

### 13. [P1012 拼数](https://www.luogu.com.cn/problem/P1012)

思路：我们把数存进字符串数组，然后以`a+b>b+a`比较，这样两数合并产生更大结果的数会被排序到前面，字符串长度一样的时候按照字典序比较，和正常比较数的结果是一样的，然后我们把排序结果直接输出就可以得到最大的数

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    int n;
    cin >> n;
    vector<string> nums(n);
    for (auto& s:nums)
    {
        cin >> s;
    }
    
    sort(nums.begin(),nums.end(),
    [](string a,string b)
    {
        return a+b>b+a;
    });
    
    for (auto& s:nums)
    {
        cout << s;
    }
}
```

### 14. [P1923 求第 k 小的数](https://www.luogu.com.cn/problem/P1923)

思路：直接用`nth_element`，我们不是算法竞赛，能快就快.jpg，注意nth_element是一种排序方法，它保证指定位置左边一定小右边一定大，而且不保证顺序，比一般排序快，我们要获取第k大(从0计)的可以用`*(nums.begin()+k)`或者`nums[k]`，使用迭代器读取的时候别忘记加括号再解引用，不然会变成`*(nums.begin())+k`，注意这题输入量很大，所以要用`ios_base::sync_with_stdio(0)`和`cin.tie(nullptr)`来加速读取

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    ios_base::sync_with_stdio(0);
    cin.tie(nullptr);
    
    int n,k;
    cin >> n >> k;
    vector<ll> nums(n);
    for (auto& i:nums)
    {
        cin >> i;
    }
    nth_element(nums.begin(),nums.begin()+k,nums.end());
    cout << *(nums.begin()+k);
    
}
```

### 15. [P1116 车厢重组](https://www.luogu.com.cn/problem/P1116)

思路：仔细读一下题目，发现和冒泡排序的原理一模一样，所以这题其实问的是冒泡排序要交换几次，考的是基本功

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int main() 
{
    int n;
    cin >> n;
    vector<int> nums(n);
    for (auto& i:nums)
    {
        cin >> i;
    }
    
    int res=0;
    for (int i=0;i<n-1;i++)
    {
        for (int j=0;j<n-1-i;j++)
        {
            if (nums[j]>nums[j+1])
            {
                swap(nums[j],nums[j+1]);
                res++;
            }
        }
    }
    cout << res;
}
```

### 16. [P1068 分数线划定](https://www.luogu.com.cn/problem/P1068)

思路：先把参与者按照分数和ID排序，然后按照排名找到分数线，输出大于等于分数线的参与者

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

struct Att
{
    int id;
    int score;    
};

int main() 
{
    int n,m;
    cin >> n >> m;
    vector<Att> atts(n);
    for (auto& i:atts)
    {
        cin >> i.id >> i.score;
    }
    
    sort(atts.begin(),atts.end(),
    [](Att& a,Att& b)
    {
        if (a.score!=b.score)
        {
            return a.score>b.score;
        } 
        return a.id<b.id;
    });
    
    int pass_rank=floor(m*1.5);
    int pass_score=atts[pass_rank-1].score;
    int res=0;
    for (auto &i:atts)
    {
        if (i.score<pass_score) break;
        res++;
    }
    
    cout << pass_score << " " << res << '\n';
    for (auto &i:atts)
    {
        if (i.score<pass_score) break;
        printf("%04d %d\n",i.id,i.score);
    }
}
```

### 17. [P1706](https://www.luogu.com.cn/problem/P1706)

思路1：用递归遍历所有位置的排列，使用`used`集合来标记每个使用过的数字

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

int n;
void dfs(int start,vector<int> nums,unordered_set<int> used)
{
    if (nums.size()==n)
    {
        for (auto& i:nums)
        {
            printf("%5d",i);
        }
        cout << '\n';
        return;
    }
    
    for (int i=1;i<=n;i++)
    {
        if (used.count(i)) continue;
        
        nums.push_back(i);
        used.insert(i); 
        dfs(i+1,nums,used);
        used.erase(i);
        nums.pop_back();
    }
    
}

int main() 
{
    cin >> n;
    vector<int> nums;
    unordered_set<int> used;
    dfs(1,nums,used);
}
```

思路2：使用`next_permutaion`输出所有组合，注意，`next_permutation`只能输出排序过的组合，按照字典序排列，没有排序过的话输出的组合会不完整

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;


int main() 
{
    int n;
    cin >> n;
    vector<int> nums(n);
    for (int i=1;i<=n;i++)
    {
        nums[i-1]=i;    
    }
    
    do
    {
        for (auto& i:nums)
        {
            printf("%5d",i);
        }
        cout << '\n';
    } while (next_permutation(nums.begin(),nums.end())); 

}
```

### 18. [P2249 查找](https://www.luogu.com.cn/problem/P2249)

思路：用二分查找`lower_bound`，注意使用这个方法必须是排过序的，返回值是第一个大于等于所给数的迭代器

|方法|作用|返回值|
|:--:|:--:|:--:|
|binary_search|查找元素是否存在|bool（是否存在）|
|lower_bound|第一个大于等于所给数的位置|迭代器|
|upper_bound|第一个大于所给数的位置|迭代器|

> `upper_bound - lower_bound`还可以查找元素出现次数

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;


int main() 
{
    ios_base::sync_with_stdio(0);
    cin.tie(nullptr);
    
    int m,q;
    cin >> m >> q;
    vector<int> nums(m);
    for (auto& i:nums)
    {
        cin >> i;
    }
    sort(nums.begin(),nums.end());
    string sep;
    while (q--)
    {
        int k;
        cin >> k;
        auto it=lower_bound(nums.begin(),nums.end(),k);
        if (it==nums.end()||*it!=k)
        {
            cout << sep << -1;
        }
        else
        {
            cout << sep << it-nums.begin()+1;
        }
        sep=" ";
    }
}
```

## 真题

### 1. [Lutece 167 a ^ b](https://cdoj.site/d/lutece/p/Lutece0167)

### 2. [最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

### 3. [兔子试毒](https://www.luogu.com.cn/problem/T158663)

### 4. [淹没岛屿](https://www.luogu.com.cn/problem/P8662)

### 5. [质因数分解](https://judge.codemao.cn/problem/2256)

### 6. [螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/)

### 7. [插队](https://www.luogu.com.cn/problem/U563768)
