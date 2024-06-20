# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==潘子轩、信科==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：动态规划，最长递降子序列



##### 代码

```python
k = int(input())
lst = list(map(int, input().split()))
dp = [0] * k
dp[0] = 1
for i in range(1, k):
    dp[i] = 1
    for j in range(0, i):
        if lst[j] >= lst[i]:
            dp[i] = max(dp[j] + 1, dp[i])
print(max(dp))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240307184731531](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307184731531.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：用栈储存盘子编号



##### 代码

```python
def tower(from_, to, num):
    if num == 1:
        trans = col_dic[from_].pop()
        print(f"{trans}:{from_}->{to}")
        col_dic[to].append(trans)
    else:
        rest = from_
        for _ in (a, b, c):
            if _ != from_ and _ != to:
                rest = _
        tower(from_, rest, num - 1)
        tower(from_, to, 1)
        tower(rest, to, num - 1)


n, a, b, c = input().split()
n = int(n)
col_dic = {a: [x for x in range(n, 0, -1)], b: [], c: []}
tower(a, c, n)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240307184842357](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307184842357.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：用队列模拟



##### 代码

```python
import queue

while True:
    n, p, m = map(int, input().split())
    if n == p == m == 0:
        break
    q = queue.Queue()
    for i in range(p - 1, p + n - 1):
        q.put(i % n + 1)
    first =True
    while n > 0:
        for _ in range(m-1):
            kid = q.get()
            q.put(kid)
        if first:
            print(q.get(), end='')
            first = False
        else:
            print(f",{q.get()}", end='')
        n -= 1
    print()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307184903230](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307184903230.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：排序，贪心



##### 代码

```python
n = int(input())
idy = 1
stu_dic = {}
for t in map(int, input().split()):
    stu_dic[idy] = t
    idy += 1
new_dic = sorted(stu_dic.items(), key=lambda x: x[1])
first = True
for item in new_dic:
    if first:
        print(item[0], end='')
        first = False
    else:
        print(f" {item[0]}", end='')
print()
aver = sum([new_dic[x][1] * (n - x - 1) for x in range(n)]) / n
print("%.2f" % aver)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307184949023](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307184949023.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：按照题目要求一步步来



##### 代码

```python
n = int(input())
pairs = [i[1:-1] for i in input().split()]
distances = [sum(map(int, i.split(','))) for i in pairs]
prices_ = list(map(int, input().split()))
q_ = [distances[x] / prices_[x] for x in range(n)]
prices = sorted(prices_)
q = sorted(q_)
if n % 2 != 0:
    mid_q = q[n // 2]
    mid_p = prices[n // 2]
else:
    mid_q = (q[n // 2] + q[n // 2 - 1]) / 2
    mid_p = (prices[n // 2] + prices[n // 2 - 1]) / 2
t_sum = 0
for i in range(n):
    if q_[i] > mid_q and prices_[i] < mid_p:
        t_sum += 1
print(t_sum)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307185019466](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307185019466.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：用字典收集，排序输出



##### 代码

```python
n = int(input())
dic = {}
for _ in range(n):
    name, q = input().split(sep='-')
    if name in dic.keys():
        dic[name].append(q)
    else:
        dic[name] = [q]
for l in dic.values():
    l.sort(key=lambda x: float(x[:len(x) - 1]) * 1000 if x[-1] == 'B' else float(x[:len(x) - 1]))
new_dic = sorted(dic.items())
for i in new_dic:
    print(f"{i[0]}: {', '.join(map(str, i[1]))}")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307185715142](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240307185715142.png)



## 2. 学习总结和收获

考试的时候没有关注时间，导致最后一题没传上去，以后应该加强时间管理。

python语法还是不熟悉，需要边查语法边做，这也耽误时间了，应该多练习，实在记不住的放在cheating-paper。

尝试使用lambda表达式，比较节省时间。



