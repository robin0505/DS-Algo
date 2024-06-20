# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

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

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：递归



代码

```python
def rc(s):
    i = 0
    ans = ''
    stack = 0
    while i < len(s):
        i0 = i
        while i < len(s) and s[i] != '(':
            i += 1
        ans += s[i0:i]
        if i == len(s):
            break
        if i < len(s) and s[i] == '(':
            i += 1
            i0 = i
            while s[i] != ')' or stack != 0:
                first = False
                if s[i] == '(':
                    stack += 1
                elif s[i] == ')':
                    stack -= 1
                i += 1
            if i < len(s):
                ans += rc(s[i0:i])
            else:
                ans += rc(s[i0:len(s)-1])
            i += 1
    return ans[::-1]


print(rc(input())[::-1])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240424203812999](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240424203812999.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：复制之前的



代码

```python
class BinaryTreeNode:
    def __init__(self, value):
        self.left = None
        self.right = None
        self.value = value


def buildTree(preorder, inorder):
    if len(preorder) == len(inorder) == 1:
        return BinaryTreeNode(preorder)
    root = BinaryTreeNode(preorder[0])
    index = inorder.find(preorder[0])
    if index != 0:
        root.left = buildTree(preorder[1:index + 1], inorder[:index])
    if index != len(inorder) - 1:
        root.right = buildTree(preorder[index + 1:], inorder[index + 1:])
    return root


def postorderTraversal(root):
    if root.left:
        postorderTraversal(root.left)
    if root.right:
        postorderTraversal(root.right)
    print(root.value, end='')


while True:
    try:
        pre, mid = input().split()
        postorderTraversal(buildTree(pre, mid))
        print()
    except EOFError:
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240425184654095](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240425184654095.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==





