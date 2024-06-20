# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by ==潘子轩、信科==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：依照题意



代码

```python
word_lst= list(map(str, input().split()))
ans_lst = []
for item in word_lst[::-1]:
    ans_lst.append(item)
print(' '.join(ans_lst))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407084649909](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407084649909.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：使用队列模拟



代码

```python
from collections import deque

m, n = map(int, input().split())
w_lst = list(map(int, input().split()))

q = deque()
ans = 0
for word in w_lst:
    if word not in q:
        ans += 1
        if len(q) == m:
            q.popleft()
            q.append(word)
        else:
            q.append(word)
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407084715116](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407084715116.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：排序之后直接选，特殊情况有点多



代码

```python
n, k = map(int, input().split())
lst = list(map(int, input().split()))

lst.sort()

if k == len(lst):
    print(lst[-1])
elif k == 0:
    if lst[0] - 1 >= 1:
        print(lst[0] - 1)
    else:
        print(-1)
elif lst[k] == lst[k - 1]:
    print(-1)
else:
    print(lst[k - 1])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407084812336](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407084812336.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：依照题意



代码

```python
class FBI:
    def __init__(self, v):
        self.value = v
        self.left = None
        self.right = None


def build(node):
    if len(node.value) <= 1:
        return
    i = len(node.value)
    le = FBI(node.value[:i//2])
    r = FBI(node.value[i//2:])
    build(le)
    build(r)
    node.left = le
    node.right = r


def post(node):
    if node.left:
        post(node.left)
    if node.right:
        post(node.right)
    if node.value == '0' * len(node.value):
        print('B', end='')
    elif node.value == '1' * len(node.value):
        print('I', end='')
    else:
        print('F', end='')


n = 2**int(input())
s = input()

R = FBI(s)
build(R)
post(R)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407084842172](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407084842172.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：用队列模拟，找到自己的小组后插入



代码

```python
from collections import deque

n = int(input())
mem_lst = []
mem_dic = {}
for _ in range(n):
    mem_lst.append(list(map(int, input().split())))
for i in range(n):
    for am in mem_lst[i]:
        mem_dic[am] = i
q = deque()
while True:
    sign = input()
    if sign == 'STOP':
        break
    elif sign == 'DEQUEUE':
        print(q[0])
        q.popleft()
    else:
        op, num = sign.split()
        num = int(num)
        if mem_dic[num] not in [mem_dic[x] for x in q]:
            q.append(num)
        else:
            i = 0
            while i < len(q):
                if mem_dic[q[i]] == mem_dic[num] and (i == len(q) - 1 or mem_dic[q[i + 1]] != mem_dic[num]):
                    break
                i += 1
            q.insert(i + 1, num)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407084934553](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407084934553.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：用字典当树



代码

```python
n = int(input())
num_dic = {}
for _ in range(n):
    lst = list(map(int, input().split()))
    num_dic[lst[0]] = lst[1:]

root_num = 0
for num in num_dic.keys():
    is_root = True
    for item in num_dic.keys():
        if num in num_dic[item]:
            is_root = False
            break
    if is_root:
        root_num = num
        break


def out_f(num):
    l = num_dic[num]
    l.append(num)
    l.sort()
    for i in l:
        if i == num:
            print(num)
            continue
        else:
            out_f(i)

out_f(root_num)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407085013038](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240407085013038.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

月考做起来整体比较轻松，但是每日选做跟不上了，期中后要赶一赶。



