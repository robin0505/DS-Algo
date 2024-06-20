# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by ==潘子轩、信科==



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：复制之前的前中转后，但是从字母到数字的改变带来了很多bug，修了半天。复制也要动脑子（



代码

```python
class BinaryTreeNode:
    def __init__(self, value):
        self.left = None
        self.right = None
        self.value = value


def build_tree(preorder, inorder):
    if len(preorder) == len(inorder) == 1:
        return BinaryTreeNode(preorder[0])
    root = BinaryTreeNode(preorder[0])
    index = inorder.index(preorder[0])
    if index != 0:
        root.left = build_tree(preorder[1:index + 1], inorder[:index])
    if index != len(inorder) - 1:
        root.right = build_tree(preorder[index + 1:], inorder[index + 1:])
    return root


def postorder_traversal(root):
    if root.left:
        postorder_traversal(root.left)
    if root.right:
        postorder_traversal(root.right)
    ans_lst.append(root.value)


n = input()
ans_lst = []
pre = list(input().split(' '))
mid = sorted(list(pre), key=lambda x: int(x))
postorder_traversal(build_tree(pre, mid))
print(' '.join(ans_lst))

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240325194918779](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240325194918779.png)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：建树，层次遍历



代码

```c++
#include <iostream>
#include <deque>

using namespace std;

struct BST {
    int value{0};
    BST *left_child{nullptr};
    BST *right_child{nullptr};
    BST *parent{nullptr};
};

void insert(BST *current_root, BST *node) {
    if (node->value == current_root->value)
        return;
    else if (node->value < current_root->value) {
        if (current_root->left_child)
            insert(current_root->left_child, node);
        else {
            current_root->left_child = node;
            node->parent = current_root;
        }
    } else {
        if (current_root->right_child)
            insert(current_root->right_child, node);
        else {
            current_root->right_child = node;
            node->parent = current_root;
        }
    }
}

int main() {
    int num;
    cin >> num;
    BST *root = new BST{num};
    while (cin >> num) {
        BST *node = new BST{num};
        insert(root, node);
    }
    deque<BST *> q;
    q.push_back(root);
    bool first{true};
    while (!q.empty()) {
        BST *w = q[0];
        q.pop_front();
        if (w->left_child) q.push_back(w->left_child);
        if (w->right_child)q.push_back(w->right_child);
        if (first) {
            cout << w->value;
            first = false;
        } else
            cout << ' ' << w->value;
    }
}
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240326165134715](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240326165134715.png)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：崩溃了，查了一个多小时，最后问题出在父节点是（i-1）/2，这下印象深刻了。



代码

```c++
#include <iostream>

using namespace std;

int size_{0};
int a[100010]{0};

void keep_heap(int pos) {
    int f = (pos - 1) >> 1;
    if (f >= 0 && a[pos] < a[f]) {
        int temp = a[pos];
        a[pos] = a[f];
        a[f] = temp;
        keep_heap(f);
    }
}

void heapify(int i) {
    if (i >= size_ - 1)
        return;
    int l{(i << 1) + 1}, r{(i << 1) + 2};
    if (l <= size_ - 1 && r <= size_ - 1) {
        int mc = a[l] < a[r] ? l : r;
        if (a[i] > a[mc]) {
            int temp = a[i];
            a[i] = a[mc];
            a[mc] = temp;
            heapify(mc);
        }
    } else if (l <= size_ - 1 && r > size_ - 1) {
        int mc = l;
        if (a[i] > a[mc]) {
            int temp = a[i];
            a[i] = a[mc];
            a[mc] = temp;
            heapify(mc);
        }
    } else if (l > size_ - 1 && r <= size_ - 1) {
        int mc = r;
        if (a[i] > a[mc]) {
            int temp = a[i];
            a[i] = a[mc];
            a[mc] = temp;
            heapify(mc);
        }
    }
}

void del() {
    a[0] = a[size_ - 1];
    size_--;
    heapify(0);
}

int main() {
    int n;
    cin >> n;
    while (n--) {
        int sign;
        cin >> sign;
        if (sign == 1) {
            cin >> a[size_++];
            keep_heap(size_ - 1);
        } else if (sign == 2) {
            cout << a[0] << endl;
            del();
        }
    }
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240326200630414](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240326200630414.png)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：照着讲义写



代码

```python
import heapq


class HuffNode:
    def __init__(self, value, ff):
        self.value = value
        self.f = ff
        self.left = None
        self.right = None
        self.parent = None

    def __cmp__(self, other):
        if self.f > other.f:
            return 1
        elif self.f < other.f:
            return -1
        elif self.f == other.f:
            return 0

    def __lt__(self, other):
        if self.f < other.f:
            return True


n = int(input())
heap = []
for i in range(n):
    v, f = input().split()
    f = int(f)
    heapq.heappush(heap, HuffNode(v, f))

leaves = heap[::]

while len(heap) > 1:
    a = heap[0]
    heap.remove(heap[0])
    heap.sort()
    b = heap[0]
    heap.remove(heap[0])
    heap.sort()
    new_node = HuffNode(None, a.f + b.f)
    new_node.left = a
    new_node.right = b
    a.parent = new_node
    b.parent = new_node
    heapq.heappush(heap, new_node)

root = heap[0]
while True:
    try:
        s = input()
        if s[0] in ['1', '0']:
            i = 0
            cnode = root
            while i < len(s):
                if s[i] == '0':
                    cnode = cnode.left
                else:
                    cnode = cnode.right
                if cnode.left is None and cnode.right is None:
                    print(cnode.value, end='')
                    cnode = root
                i += 1
            print()
        else:
            i = 0
            while i < len(s):
                bin_lst = ''
                l_node = None
                for item in leaves:
                    if item.value == s[i]:
                        l_node = item
                cnode = l_node
                while cnode and cnode.parent:
                    if cnode is None:
                        break
                    if cnode == cnode.parent.left:
                        bin_lst += '0'
                    else:
                        bin_lst += '1'
                    if cnode.parent is None:
                        break
                    else:
                        cnode = cnode.parent
                i += 1
                print(bin_lst[::-1], end='')
            print()
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240326212352334](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240326212352334.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：本来想自己写一遍，但根本写不明白，最后还是变成了对着标准代码抄一遍。



代码

```python
class AVLNode:
    def __init__(self, value):
        self.value = value
        self.height = 1
        self.bf = 0
        self.left = None
        self.right = None


class AVLTree:
    def __init__(self):
        self.root = None

    def insert(self, value):
        if self.root is None:
            self.root = AVLNode(value)
        else:
            self.root = self._insert(self.root, value)

    def _insert(self, node, value):
        if node is None:
            return AVLNode(value)
        elif node.value > value:
            node.left = self._insert(node.left, value)
        elif node.value < value:
            node.right = self._insert(node.right, value)

        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))
        node.bf = self._get_balance(node)

        if node.bf > 1:
            if value >= node.left.value:
                node.left = self.rotate_left(node.left)
                return self.rotate_right(node)
            else:
                return self.rotate_right(node)

        elif node.bf < -1:
            if value <= node.right.value:
                node.right = self.rotate_right(node.right)
                return self.rotate_left(node)
            else:
                return self.rotate_left(node)

        return node

    def _get_height(self, node):
        if node is None:
            return 0
        else:
            return node.height

    def _get_balance(self, node):
        if node is None:
            return 0
        else:
            return self._get_height(node.left) - self._get_height(node.right)

    def rotate_left(self, node):
        new_root = node.right
        temp = new_root.left
        new_root.left = node
        node.right = temp
        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))
        new_root.height = 1 + max(self._get_height(new_root.left), self._get_height(new_root.right))
        return new_root

    def rotate_right(self, node):
        new_root = node.left
        temp = new_root.right
        new_root.right = node
        node.left = temp
        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))
        new_root.height = 1 + max(self._get_height(new_root.left), self._get_height(new_root.right))
        return new_root

    def pre_order(self, node):
        if node is None:
            return []
        return [node.value] + self.pre_order(node.left) + self.pre_order(node.right)


n = int(input())
arr = list(map(int, input().split()))

avl = AVLTree()
for num in arr:
    avl.insert(num)

print(' '.join(map(str, avl.pre_order(avl.root))))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240327190714695](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240327190714695.png)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：在网上找了一个并查集的板子，比自己写的要快一些。找到根节点的数量就是宗教的最多种数。



代码

```c++
#include <iostream>

using namespace std;

const int N = 50001;                    //指定并查集所能包含元素的个数（由题意决定）
int pre[N];                        //存储每个结点的前驱结点
int rank_[N];                        //树的高度
void init(int n)                    //初始化函数，对录入的 n个结点进行初始化
{
    for (int i = 0; i < n; i++) {
        pre[i] = i;                //每个结点的上级都是自己
        rank_[i] = 1;                //每个结点构成的树的高度为 1
    }
}

int find(int x)                    //改进查找算法：完成路径压缩，将 x的上级直接变为根结点，那么树的高度就会大大降低
{
    if (pre[x] == x) return x;        //递归出口：x的上级为 x本身，即 x为根结点
    return pre[x] = find(pre[x]);   //此代码相当于先找到根结点 rootx，然后 pre[x]=rootx
}

bool isSame(int x, int y)            //判断两个结点是否连通
{
    return find(x) == find(y);    //判断两个结点的根结点（即代表元）是否相同
}

bool join(int x, int y) {
    x = find(x);                        //寻找 x的代表元
    y = find(y);                        //寻找 y的代表元
    if (x == y) return false;            //如果 x和 y的代表元一致，说明他们共属同一集合，则不需要合并，返回 false，表示合并失败；否则，执行下面的逻辑
    if (rank_[x] > rank_[y]) pre[y] = x;        //如果 x的高度大于 y，则令 y的上级为 x
    else                                //否则
    {
        if (rank_[x] == rank_[y]) rank_[y]++;    //如果 x的高度和 y的高度相同，则令 y的高度加1
        pre[x] = y;                        //让 x的上级为 y
    }
    return true;                        //返回 true，表示合并成功
}

int main() {
    int c{1};
    while (true) {
        int n, m;
        cin >> n >> m;
        if (n == 0)
            break;
        init(n);
        while (m--) {
            int i, j;
            cin >> i >> j;
            join(i, j);
        }
        int ans{0};
        for (int i{0}; i < n; i++) {
            if (pre[i] == i) {
                ans++;
            }
        }
        cout << "Case " << c++ << ": " << ans << endl;
    }
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240327093244931](/Users/panzixuan/Library/Application Support/typora-user-images/image-20240327093244931.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

这几个数据结构真的有点难，每个都改了好长时间，希望逐渐熟练之后能好一些。



