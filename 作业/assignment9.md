# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

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

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：题怎么说的我怎么写



代码

```python
class Node:
    def __init__(self, sh):
        self.h1 = sh
        self.h2 = 0
        self.children = []
        self.p = None


root = Node(0)
s = input()
cu = root
for c in s:
    if c == 'd':
        nn = Node(cu.h1 + 1)
        cu.children.append(nn)
        nn.p = cu
        cu = nn
    if c == 'u':
        cu = cu.p


def th(n):
    for i in range(len(n.children)):
        n.children[i].h2 = n.h2 + i + 1
        if n.children[i].children:
            th(n.children[i])


th(root)


def pre(n):
    if n.children:
        return max([pre(x) for x in n.children])
    else:
        return n.h2


def pre0(n):
    if n.children:
        return max([pre0(x) for x in n.children])
    else:
        return n.h1


print(f"{pre0(root)} => {pre(root)}")
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240417161654007](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240417161654007.png)



### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：



代码

```python
class Node:
    def __init__(self, value):
        self.v = value
        self.lf = None
        self.rt = None
        self.p = None


f = True
r = None
cu = None
for c in input():
    if f:
        r = Node(c)
        cu = r
        f = False
    else:
        nn = Node(c)
        while cu.rt:
            cu = cu.p
        if not cu.lf:
            cu.lf = nn
            nn.p = cu
        elif not cu.rt:
            cu.rt = nn
            nn.p = cu
        if c != '.':
            cu = nn


def pre(n):
    if n.lf:
        pre(n.lf)
    if n.v != '.':
        print(n.v, end='')
    if n.rt:
        pre(n.rt)


def pos(n):
    if n.lf:
        pos(n.lf)
    if n.rt:
        pos(n.rt)
    if n.v != '.':
        print(n.v, end='')


pre(r)
print()
pos(r)
```



代码运行截图 ==（至少包含有"Accepted"）==





### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：空间换时间



代码

```python
s = []
m = []
while True:
    try:
        ins = input()
        if ins == 'pop' and s:
            s.pop()
            m.pop()
        elif ins == 'min' and s:
            print(m[-1])
        elif ins[:4] == 'push':
            a = int(ins[5:])
            if m:
                m.append(min(m[-1], a))
            else:
                m.append(a)
            s.append(a)
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240417191524364](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240417191524364.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：dfs



代码

```python
dx = [1, 1, 2, 2, -1, -1, -2, -2]
dy = [-2, 2, 1, -1, -2, 2, -1, 1]
t = int(input())
for _ in range(t):
    n, m, x, y = map(int, input().split())
    matrix = [[0 for _ in range(m)] for _ in range(n)]
    matrix[x][y] = 1


    def valuable(mtx, ox, oy):
        return 0 <= ox < n and 0 <= oy < m and mtx[ox][oy] == 0


    def dfs(mtx, bx, by):
        end = True
        for i in mtx:
            for j in i:
                if j != 1:
                    end = False
                    break
        if end:
            return 1
        if not end:
            for i in range(8):
                if valuable(mtx, bx + dx[i], by + dy[i]):
                    end = False
                    break
        if end:
            return 0
        result = 0
        for i in range(8):
            nx = bx + dx[i]
            ny = by + dy[i]
            if valuable(mtx, nx, ny):
                mtx = mtx
                mtx[nx][ny] = 1
                result += dfs(mtx, nx, ny)
                mtx[nx][ny] = 0
        return result


    print(dfs(matrix, x, y))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240418192437093](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240418192437093.png)



### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：找最近应该用bfs。并且可以用词桶来空间换时间。



代码

```python
from collections import deque


class WordNode:
    def __init__(self, _v):
        self.v = _v
        self.nbr = []
        self.pv = None


n = int(input())
word_list = []
word_ton = [{}, {}, {}, {}]

for _ in range(n):
    new_word = WordNode(input())
    for i in range(4):
        if i == 3:
            the_key = new_word.v[:3]
        else:
            the_key = new_word.v[:i] + new_word.v[i + 1:]
        if the_key in word_ton[i].keys():
            word_ton[i][the_key].append(new_word)
        else:
            word_ton[i][the_key] = [new_word]
    word_list.append(new_word)

for i in range(4):
    for a_ton in word_ton[i].values():
        for ii in range(len(a_ton)):
            for jj in range(ii + 1, len(a_ton)):
                a_ton[ii].nbr.append(a_ton[jj])
                a_ton[jj].nbr.append(a_ton[ii])


begin_word_s, end_word_s = input().split()

used_word = {}
bw = None
for word in word_list:
    if word.v == begin_word_s:
        bw = word
    used_word[word.v] = 0
used_word[begin_word_s] = 1

q = deque()
q.append(bw)
final = None
ans = []
while q:
    a = q[0]
    b = False
    for i in a.nbr:
        if used_word[i.v] == 0:
            i.pv = a
            q.append(i)
            used_word[i.v] = 1
        if i.v == end_word_s:
            final = i
            b = True
            break
    q.popleft()
    if b:
        break
if not final:
    print('NO')
else:
    while final:
        ans.append(final.v)
        final = final.pv
print(' '.join(ans[::-1]))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240419100933714](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240419100933714.png)



### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：启发式搜索



代码

```python
dx = [1, 1, 2, 2, -1, -1, -2, -2]
dy = [-2, 2, 1, -1, -2, 2, -1, 1]

n = int(input())
x, y = map(int, input().split())
m = n
matrix = [[0 for _ in range(m)] for _ in range(n)]
matrix[x][y] = 1


def valuable(mtx, ox, oy):
    return 0 <= ox < n and 0 <= oy < m and mtx[ox][oy] == 0


def dfs(mtx, bx, by, length):
    end = False
    if length == n ** 2:
        return True
    choice = []
    if not end:
        for i in range(8):
            nx = bx + dx[i]
            ny = by + dy[i]
            if valuable(mtx, nx, ny):
                end = False
                temp_sum = 0
                mtx[nx][ny] = 1
                for j in range(8):
                    nnx = nx + dx[j]
                    nny = ny + dy[j]
                    if valuable(mtx, nnx, nny):
                        temp_sum += 1
                choice.append([i, temp_sum])
                mtx[nx][ny] = 0

    if end:
        return False

    choice.sort(key=lambda xx: xx[1])
    for i in [xx[0] for xx in choice]:
        nx = bx + dx[i]
        ny = by + dy[i]
        mtx[nx][ny] = 1
        if dfs(mtx, nx, ny, length + 1):
            return True
        mtx[nx][ny] = 0
    return False


if not dfs(matrix, x, y, 1):
    print('fail')
else:
    print('success')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240419105737947](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240419105737947.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

感觉难度很大，自己不太容易想出来，需要看书才能写出来。



