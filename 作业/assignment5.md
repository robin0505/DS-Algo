# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

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

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：dfs



代码

```c++
#include <iostream>

using namespace std;

int leaves{0};

int dfs(int lst[][2], int p) {
    if (lst[p][0] == -1 && lst[p][1] == -1) {
        leaves++;
        return 0;
    } else if (lst[p][0] == -1 && lst[p][1] != -1)
        return dfs(lst, lst[p][1]) + 1;
    else if (lst[p][0] != -1 && lst[p][1] == -1)
        return dfs(lst, lst[p][0]) + 1;
    else
        return max(dfs(lst, lst[p][1]), dfs(lst, lst[p][0])) + 1;
}

int main() {
    int n;
    int lst[100][2];
    cin >> n;
    for (int i{0}; i < n; i++)
        cin >> lst[i][0] >> lst[i][1];
    int root{0};
    for (int i{0}; i < n; i++) {
        bool isRoot{true};
        for (int j{0}; j < n; j++)
            if (lst[j][0] == i || lst[j][1] == i)
                isRoot = false;
        if (isRoot) {
            root = i;
            break;
        }
    }
    cout << dfs(lst, root) << ' ' << leaves;
}
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240319175057066](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240319175057066.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：不知道为什么突然喜欢写c++了，感觉c的指针比python的引用要好用。



代码

```c++
#include <iostream>

using namespace std;

struct Tree_node {
    char value{'A'};
    Tree_node *lst[26]{nullptr};
    Tree_node *father{nullptr};
    int size{0};
};

void pre_order(Tree_node *root){
    int it{0};
    cout << root->value;
    while (root->lst[it]!= nullptr){
        pre_order(root->lst[it]);
        it++;
    }
}

void pos_order(Tree_node *root){
    int it{0};
    while (root->lst[it]!= nullptr){
        pos_order(root->lst[it]);
        it++;
    }
    cout << root->value;
}

int main() {
    char s[500]{0};
    cin >> s;

    int l{0};
    while (s[l] != '\0')
        l++;

    auto *root = new Tree_node{s[0]};
    Tree_node *current = root;
    int index{2};
    while (index < l)
        if (isalpha(s[index])) {
            auto *new_node = new Tree_node{s[index]};
            current->lst[current->size++] = new_node;
            new_node->father = current;
            index++;
            if (s[index] == '(') {
                current = new_node;
                index++;
            } else if (s[index] == ')') {
                current = current->father;
                index++;
            }
        } else if (s[index] == ')'){
            current = current->father;
            index++;
        }else
            index++;

    pre_order(root);
    cout << endl;
    pos_order(root);
}
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240319200553587](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240319200553587.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：构建文件树，写得有点慢



代码

```python
class FileTreeNode:
    def __init__(self, nam):
        self.name = nam
        self.children = []
        self.parent = None

    def is_file(self):
        if self.name[0] == 'd':
            return True
        else:
            return False


def build_tree(current_root):
    while True:
        s = input()
        if s == ']' or s == '*':
            break
        new_node = FileTreeNode(s)
        current_root.children.append(new_node)
        new_node.parent = current_root
        if s[0] == 'd':
            build_tree(new_node)


def output_tree(current_root, level):
    print('|     ' * level + current_root.name)
    current_root.children.sort(key=lambda x: 'a' if x.is_file() else x.name)
    contain_file = False
    for child in current_root.children:
        if child.is_file():
            contain_file = True
    if contain_file:
        for child in current_root.children:
            if child.is_file():
                output_tree(child, level + 1)
            else:
                output_tree(child, level)
    else:
        for child in current_root.children:
            output_tree(child, level)


set_num = 0
end = False
while True:
    root = FileTreeNode('ROOT')
    while True:
        input_line = input()
        if input_line == '*':
            set_num += 1
            break
        if input_line == '#':
            end = True
            break
        level_one = FileTreeNode(input_line)
        root.children.append(level_one)
        level_one.parent = root
        if input_line[0] == 'd':
            build_tree(level_one)
    if end:
        break
    print(f"DATA SET {set_num}:")
    output_tree(root, 0)
    print()

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240319211356374](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240319211356374.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：按照提示建树



代码

```c++
#include <iostream>
#include <deque>

using namespace std;

struct binary_tree_node {
    char value{'a'};
    binary_tree_node *left_child{nullptr};
    binary_tree_node *right_child{nullptr};
};

int main() {
    int n;
    cin >> n;
    while (n--) {
        char s[101]{0};
        binary_tree_node *stack[101]{nullptr};
        int size{0};
        cin >> s;
        int i{0};
        binary_tree_node *root{nullptr};
        while (s[i] != '\0') {
            if (islower(s[i])) {
                auto new_node = new binary_tree_node{s[i]};
                stack[size++] = new_node;
            } else if (isupper(s[i])) {
                auto new_node = new binary_tree_node{s[i]};
                if (s[i + 1] == '\0')
                    root = new_node;
                new_node->right_child = stack[--size];
                new_node->left_child = stack[--size];
                stack[size++] = new_node;
            }
            i++;
        }
        deque<binary_tree_node *> q;
        char ans[101]{0};
        int ans_size{0};
        q.push_back(root);
        while (!q.empty()) {
            auto head = q[0];
            ans[ans_size++] = head->value;
            if (head->left_child)
                q.push_back(head->left_child);
            if (head->right_child)
                q.push_back(head->right_child);
            q.pop_front();
        }
        for (int w{ans_size - 1}; w >= 0; w--)
            cout << ans[w];
        cout << endl;
    }
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240320141827824](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240320141827824.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：递归



代码

```python
class BinaryTreeNode:
    def __init__(self, value):
        self.left = None
        self.right = None
        self.value = value


def build_tree(sub_mid, sub_post):
    if len(sub_mid) == len(sub_post) == 1:
        return BinaryTreeNode(sub_mid)
    root = sub_post[-1]
    sep = sub_mid.find(root)
    _root = BinaryTreeNode(root)
    if sep > 0:
        _root.left = build_tree(sub_mid[:sep], sub_post[:sep])
    if sep < len(sub_mid) - 1:
        _root.right = build_tree(sub_mid[sep + 1:], sub_post[sep:-1])
    return _root


def preorder(root):
    print(root.value, end='')
    if root.left:
        preorder(root.left)
    if root.right:
        preorder(root.right)


mid = input()
post = input()
t_root = build_tree(mid, post)
preorder(t_root)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240320142955207](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240320142955207.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：同上



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
        pre = input()
        mid = input()
        postorderTraversal(buildTree(pre, mid))
        print()
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240320144825526](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240320144825526.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

逐渐熟练了二叉树的使用



