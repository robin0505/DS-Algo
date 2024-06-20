# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by ==潘子轩、信科==



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

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

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：类似斐波那契数列



##### 代码

```python
a, b, c = 0, 1, 1
n = int(input())
for i in range(n-2):
    a, b, c = b, c, a+b+c
print(c)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240220160934026](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240220160934026.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：用两个坐标同时寻找



##### 代码

```python
s = input()
s0 = 'hello'
i, i0 = 0, 0
while i < len(s) and i0 < len(s0):
    if s[i] == s0[i0]:
        i += 1
        i0 += 1
    else:
        i += 1
if i0 == 5:
    print('YES')
else:
    print('NO')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240220165951202](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240220165951202.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：选择元音直接输出



##### 代码

```python
s = input()
for c in s:
    if c not in 'AOYEUIaoyeui':
        print('.'+c.lower(), end='')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220172654320](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240220172654320.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：穷举



##### 代码

```python
def is_prime(n):
    i = 2
    while i * i <= n:
        if n % i == 0:
            return False
        i += 1
    return True


s = int(input())
for a in range(1, s//2):
    if is_prime(a) and is_prime(s-a):
        print(a, s-a)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240220173846565](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240220173846565.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：



##### 代码

```python
import re

input_str = input().strip()
terms = re.findall(r'(\d*)n\^(\d+)', input_str)
max_upper = 0
for term in terms:
    a, b = term
    if a == '':
        a = 0
    else:
        a = int(a)
    if a != 0:
        max_upper = max(max_upper, int(b))
print('n^' + str(max_upper))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240221224931516](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240221224931516.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：用字典计票



##### 代码

```python
num_lst = map(int, input().split())
count_dic = {}
for i in num_lst:
    if i not in count_dic:
        count_dic[i] = 1
    else:
        count_dic[i] += 1
max_votes = max(count_dic.values())
winner = [key for key, value in count_dic.items() if value == max_votes]
print(" ".join(map(str, sorted(winner))))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240221231715953](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240221231715953.png)



## 2. 学习总结和收获

初步熟悉了python语法

学习了输入输出的一些方法

学习了正则表达式





