# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ==潘子轩、信科==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：模拟



代码

```python
import collections

t = int(input())
for _ in range(t):
    n = int(input())
    q = collections.deque()
    for __ in range(n):
        typ, elem = map(int, input().split())
        if typ == 1:
            q.appendleft(elem)
        elif typ == 2:
            if elem == 0:
                q.pop()
            elif elem == 1:
                q.popleft()
    if len(q) != 0:
        q.reverse()
        print(' '.join(map(str, q)))
    else:
        print('NULL')

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240312194427742](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240312194427742.png)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：从后到前入栈



代码

```python
lst = list(input().split())
lst.reverse()
num = []
for sym in lst:
    if sym[0].isdigit():
        num.append(float(sym))
    else:
        a = num.pop()
        b = num.pop()
        if sym == '+':
            num.append(a+b)
        elif sym == '-':
            num.append(a-b)
        elif sym == '*':
            num.append(a*b)
        elif sym == '/':
            num.append(a/b)
print("%6f\n" %num[0])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240312201626471](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240312201626471.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：看的书上，然后自己默写一遍



代码

```python
n = int(input())
op_dict = {'+': 1, '-': 1, '*': 3, '/': 3, '(': 0}
for _ in range(n):
    input_expr = input()
    expr = []
    i = 0
    while i < len(input_expr):
        if input_expr[i].isdigit() or input_expr[i] == '.':
            num = input_expr[i]
            i += 1
            while i < len(input_expr) and (input_expr[i].isdigit() or input_expr[i] == '.'):
                num += input_expr[i]
                i += 1
            num = float(num)
            if num.is_integer():
                expr.append(int(num))
            else:
                expr.append(num)
        elif input_expr[i] in '+-*/()':
            expr.append(input_expr[i])
            i += 1
        else:
            i += 1
    op_stack = []
    ans_expr = []
    for item in expr:
        if type(item) is float or type(item) is int:
            ans_expr.append(item)
        elif item in '+-*/':
            if len(op_stack) > 0:
                while len(op_stack) > 0 and op_dict[op_stack[-1]] >= op_dict[item]:
                    ans_expr.append(op_stack.pop())
            op_stack.append(item)
        elif item == '(':
            op_stack.append('(')
        elif item == ')':
            a = op_stack.pop()
            while a != '(':
                ans_expr.append(a)
                a = op_stack.pop()
    while len(op_stack) > 0:
        ans_expr.append(op_stack.pop())
    print(' '.join(map(str, ans_expr)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240313000140488](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240313000140488.png)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：模拟，但是数据很可恶，改了半天



代码

```python
x = input()
while True:
    try:
        s = input()
        if len(s) != len(x):
            print('NO')
            continue
        s_stack = []
        i = 0
        legal = True
        for char in s:
            if char not in s_stack:
                while i < len(x) and x[i] != char:
                    s_stack.append(x[i])
                    i += 1
                if i >= len(x):
                    legal = False
                    print('NO')
                    break
                i += 1
            elif s_stack[-1] == char:
                s_stack.pop()
            else:
                if s_stack[-1] != char:
                    print('NO')
                    legal = False
                    break
        if legal:
            print('YES')

    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240313155224360](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240313155224360.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：不想建树，直接用的深搜



代码

```python
n = int(input())
tree = [[-1, -1] for _ in range(n + 1)]
for i in range(1, n + 1):
    tree[i][0], tree[i][1] = map(int, input().split())


def dfs(c):
    if tree[c][0] == -1 and tree[c][1] == -1:
        return 1
    elif tree[c][0] == -1 and tree[c][1] != -1:
        return dfs(tree[c][1]) + 1
    elif tree[c][0] != -1 and tree[c][1] == -1:
        return dfs(tree[c][0]) + 1
    else:
        return max(dfs(tree[c][0]), dfs(tree[c][1])) + 1


print(dfs(1))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240313183220391](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240313183220391.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：用归并。但是因为memset卡了好长时间，之前不知道这个东西也会耗这么长时间。



代码

```c++
#include <iostream>
#include <cstring>
#include <cmath>

using namespace std;

long long int a[500001]{0};
long long int temp[500001]{0};
long long int ans{0};
int n;

void sort(int begin, int end) {
    if (begin >= end)
        return;
    int mid = (begin + end) / 2;
    int i{begin}, j{mid + 1}, k{begin};
    sort(i, mid);
    sort(mid + 1, end);
    while (i <= mid && j <= end) {
        if (a[i] <= a[j]) temp[k++] = a[i++];
        else {
            temp[k++] = a[j++];
            ans += j - k;
        }
    }
    while (i <= mid) temp[k++] = a[i++];
    while (j <= end) temp[k++] = a[j++];
    for (int q{begin}; q <= end; q++) a[q] = temp[q];
}


int main() {
    while (true) {
        ans = 0;
        cin >> n;
        if (n == 0)
            break;
        for (int i{0}; i < n; i++)
            cin >> a[i];
        sort(0, n - 1);
        cout << ans << endl;
    }
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240314205505035](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240314205505035.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

复习了排序算法、栈、队列

借助题目学习了树的相关知识，但还不太会用



