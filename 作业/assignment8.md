# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

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

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：依题意



代码

```python
class Vertex:
    def __init__(self, k):
        self.key = k
        self.neighbour = []


class Graph:
    def __init__(self):
        self.vertexes = {}


lpg = Graph()
n, m = map(int, input().split())
for i in range(n):
    lpg.vertexes[i] = Vertex(i)
for _ in range(m):
    a, b = map(int, input().split())
    lpg.vertexes[a].neighbour.append(b)
    lpg.vertexes[b].neighbour.append(a)

D = [[0 for __ in range(n)] for _ in range(n)]
A = [[0 for __ in range(n)] for _ in range(n)]
L = [[0 for __ in range(n)] for _ in range(n)]
for i in range(n):
    D[i][i] = len(lpg.vertexes[i].neighbour)

for i in range(n):
    for j in range(n):
        if i in lpg.vertexes[j].neighbour:
            A[i][j] = 1

for i in range(n):
    for j in range(n):
        L[i][j] = D[i][j] - A[i][j]

for i in range(n):
    print(' '.join(map(str, L[i])))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240413151332574](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240413151332574.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：忽略了全是.的情况，查死我了



代码

```python
dx = [0, 0, 1, 1, 1, -1, -1, -1]
dy = [1, -1, -1, 0, 1, -1, 0, 1]
t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = [[0 for __ in range(m)] for _ in range(n)]
    for i in range(n):
        ipt = list(input())
        for j in range(m):
            if ipt[j] == 'W':
                a[i][j] = 1
    area = []


    def dfs(x, y):
        result = 1
        a[x][y] = 0
        for d in range(8):
            x1 = x + dx[d]
            y1 = y + dy[d]
            if (0 <= x1 < n and 0 <= y1 < m) and a[x1][y1] == 1:
                result += dfs(x1, y1)
        return result


    for i in range(n):
        for j in range(m):
            if a[i][j] == 1:
                area.append(dfs(i, j))

    if area:
        print(max(area))
    else:
        print(0)
```



代码运行截图 ==（至少包含有"Accepted"）==





### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：跟上道题基本一样



代码

```python
class Vertex:
    def __init__(self, k, w):
        self.key = k
        self.weight = w
        self.neighbour = []


n, m = map(int, input().split())
graph = []
wl = list(input().split())
for i in range(n):
    graph.append(Vertex(i, int(wl[i])))
for _ in range(m):
    u, v = map(int, input().split())
    graph[u].neighbour.append(v)
    graph[v].neighbour.append(u)

sw = []
counted = []


def dfs(vt):
    result = graph[vt].weight
    counted.append(vt)
    for nvt in graph[vt].neighbour:
        if nvt not in counted:
            result += dfs(nvt)
    return result


for item in graph:
    if item.key not in counted:
        sw.append(dfs(item.key))

if sw:
    print(max(sw))
else:
    print(0)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240413162851466](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240413162851466.png)



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：分治的思想



代码

```c++
#include <iostream>
#include <algorithm>

using namespace std;
int m[4001][4]{0};
int mem01[4001 * 4001]{0};
int mem23[4001 * 4001]{0};

int main() {
    int n, ans{0}, k{0};
    cin >> n;
    for (int i{0}; i < n; i++)
        for (int j{0}; j < 4; j++)
            cin >> m[i][j];
    for (int i{0}; i < n; i++)
        for (int j{0}; j < n; j++) {
            mem01[k] = m[i][0] + m[j][1];
            mem23[k] = m[i][2] + m[j][3];
            k++;
        }
    sort(mem01, mem01 + k);
    sort(mem23, mem23 + k);
    int right{k - 1};
    for (int left{0}; left < k; left++) {
        while (right >= 0 and mem01[left] + mem23[right] > 0)
            right--;
        if (mem01[left] + mem23[right] < 0)
            continue;
        int temp{right};
        while (right >= 0 and mem01[left] + mem23[right] == 0){
            right--;
            ans++;
        }
        right = temp;
    }
    cout << ans;
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240413175208092](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240413175208092.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：学了一下Trie，然后写得乱七八糟，到处打补丁，还是得最开始就思路清晰。



代码

```python
class Node:
    def __init__(self, value):
        self.num = value
        self.is_end = False
        self.children = []


t = int(input())
for _ in range(t):
    ans = True
    n = int(input())
    root = Node(-1)
    for __ in range(n):
        p = list(map(int, input()))
        cu = root
        counter = 0
        for num in p:
            counter += 1
            c = False
            for x in cu.children:
                if num == x.num:
                    c = True
                    if x.is_end:
                        ans = False
                        break
                    if counter == len(p):
                        ans = False
                        break
                    cu = x
                    break
            if c:
                continue
            new_node = Node(num)
            cu.children.append(new_node)
            cu = new_node
            if counter == len(p):
                new_node.is_end = True
    if ans:
        print('YES')
    else:
        print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240413190324952](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240413190324952.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：理解题意，把逻辑捋清楚，然后就越界了，查了半天，后来改成10000无脑过了



代码

```python
from collections import deque


class Bt:
    def __init__(self, value):
        self.v = value
        self.lf = None
        self.rt = None
        self.p = None
        self.h = 0


n = int(input())
sl = list(input().split())
if len(sl) == 1:
    print(sl[0][0])
else:
    root = Bt(sl[0][0])
    root.h = 1
    cu = root
    for s in sl[1:]:
        nn = Bt(s[0])
        while cu.rt:
            cu = cu.p
        if not cu.lf:
            cu.lf = nn
        elif not cu.rt:
            cu.rt = nn
        nn.p = cu
        if s[1] == '0':
            cu = nn


    def sh(node):
        if node.rt:
            node.rt.h = node.h
            sh(node.rt)
        if node.lf:
            node.lf.h = node.h + 1
            sh(node.lf)

    sh(root)
    q = deque()
    q.append(root)
    ad = {}
    for i in range(1, 10000):
        ad[i] = []
    while q:
        cu2 = q[0]
        while cu2:
            if cu2.lf:
                q.append(cu2.lf)
            if cu2.v != '$':
                ad[cu2.h].append(cu2.v)
            cu2 = cu2.rt
        q.popleft()

    print(' '.join([' '.join(x[::-1]) for x in ad.values()]))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240413204118030](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240413204118030.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

由于期中考试，很久没写代码了，手生导致写了好长好长时间，还是要多练。



