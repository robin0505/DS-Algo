# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==同学的姓名、院系==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/



思路：教材



##### 代码

```python
def gcd(m, n):
    while m % n != 0:
        m, n = n, m % n
    return n


class Fraction:
    def __init__(self, top, bottom):
        self.num = top
        self.den = bottom

    def __str__(self):
        return f'{self.num}/{self.den}'

    def __add__(self, other_fraction):
        new_num = self.num * other_fraction.den + self.den * other_fraction.num
        new_den = self.den * other_fraction.den
        cmmn = gcd(new_num, new_den)
        return Fraction(new_num // cmmn, new_den // cmmn)

    def __eq__(self, other):
        first_num = self.num * other.den
        second_num = self.den * other.num
        return first_num == second_num


a = list(map(int, input().split()))
b = Fraction(a[0], a[1])
c = Fraction(a[2], a[3])
print(b+c)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240229192733213](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240229192733213.png)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：按单位重量价值降序取



##### 代码

```python
n, w_max = map(int, input().split())
bags = []
for i in range(n):
    v, w = map(int, input().split())
    bags.append([v / w, v, w])
ans_sum = 0
bags.sort(reverse=True)
for a_bag in bags:
    if a_bag[2] <= w_max and w_max > 0:
        ans_sum += a_bag[1]
        w_max -= a_bag[2]
    elif a_bag[2] > w_max > 0:
        ans_sum += w_max * a_bag[0]
        w_max = 0
    elif w_max <= 0:
        break
print("%.1f" % ans_sum)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240227163742135](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240227163742135.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：升序找时间，降序减血



##### 代码

```python
nCases = int(input())
for _ in range(nCases):
    n, m, b = map(int, input().split())
    alive = True
    skills = {}
    for i in range(n):
        t, x = map(int, input().split())
        if t not in skills.keys():
            skills[t] = []
        skills[t].append(x)
    for a_time in sorted(skills.keys()):
        skills[a_time] = sorted(skills[a_time], reverse=True)
        limit = 0
        while b > 0 and limit < m and limit < len(skills[a_time]):
            b -= skills[a_time][limit]
            limit += 1
            if b <= 0:
                print(a_time)
                alive = False
                break
        if not alive:
            break
    if alive:
        print('alive')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240227174217546](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240227174217546.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：用筛法和c语言节省时间



##### 代码

```c
#include <cmath>
#include <stdio.h>
#include <cstring>
using namespace std;
typedef long long ll;
const int maxn = 1e6 + 5;
int a[maxn + 1];
void isprime() {
    a[0] = a[1] = 1;
    memset(a, 0, sizeof(a));
    for (int i = 2; i <= maxn; i++) {
        if (!a[i]) {
            for (int j = i + i; j <= maxn; j += i) {
                a[j] = 1;
            }
        }
    }
}

int main() {
    int t;
    scanf("%d", &t);
    isprime();
    while (t--) {
        ll n;
        scanf("%I64d", &n);
        ll x = sqrt(n);
        if (x > 1 && x * x == n && a[x] == 0)
            printf("YES\n");
        else printf("NO\n");
    }
    return 0;
}

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240227203357386](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240227203357386.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：只需要从一头找到第一个不整除的数，然后取最大



##### 代码

```c++
#include <iostream>
#include <cstring>
#include <cmath>
 
using namespace std;
int lst[100001];
 
int main() {
    int t;
    cin >> t;
    while (t--) {
        int n, x, sum{0};
        bool judge1{true};
        memset(lst, 0, sizeof lst);
        cin >> n >> x;
        for (int i{1}; i <= n; i++) {
            cin >> lst[i];
            sum += lst[i];
            if (lst[i] % x != 0)
                judge1 = false;
        }
        if (sum % x != 0) {
            cout << n << endl;
            continue;
        }
        if (judge1) {
            cout << -1 << endl;
            continue;
        }
        int b{1}, e{n};
        while (lst[b]%x==0)
            b++;
        while (lst[e]%x==0)
            e--;
        cout << max(n-b, e-1) << endl;
    }
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240229192102980](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240229192102980.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：筛法



##### 代码

```python
import math


N = 10000
s = [True] * N
p = 2
while p * p <= N:
    if s[p]:
        for i in range(p * 2, N, p):
            s[i] = False
    p += 1


def is_t_primes(num):
    if not math.sqrt(num).is_integer():
        return False
    elif num == 1:
        return False
    else:
        sqrt_num = int(math.sqrt(num))
        if s[sqrt_num]:
            return True
        else:
            return False


m, n = map(int, input().split())
for i in range(m):
    grade_lst = list(map(int, input().split()))
    the_sum = 0
    for grade in grade_lst:
        if is_t_primes(grade):
            the_sum += grade
    if the_sum == 0:
        print(0)
        continue
    print("{:.2f}".format(the_sum / len(grade_lst)))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240227211433063](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240227211433063.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

熟悉了关于素数的算法，了解了筛法的知识

复习了一下C++语法，感觉python有点慢，暂时打算在机考用C++

