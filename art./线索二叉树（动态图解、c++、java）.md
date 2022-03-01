
<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 线索二叉树概述
                                                                         
二叉树是非线性数据结构，而遍历序列是线性序列，二叉树遍历实际上是将一个非线性结构进行线性化的操作。

根据线性序列的特性，除了第一个元素外，每一个节点都有唯一的前驱，除了最后一个元素外，每一个节点都有唯一的后继。

根据遍历序列的不同，每个节点的前驱和后继也不同。

采用二叉链表存储时，只记录了左、右孩子的信息，无法直接得到每个节点的前驱和后继。

## 线索二叉树存储结构
                                                                         
二叉树采用二叉链表存储时，每个节点有两个指针域。

如果二叉链表有 n 个节点，则一共有 2n 个指针域，其中 `n-1 个是实指针，其余 n+1 个都是空指针`。

二叉树有 n-1 个分支，每个分支对应一个实指针。如图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/af18bbff37204ec184117fc0a54ea99a.png)

`可以充分利用空指针记录节点的前驱或后继信息，从而加快查找节点前驱和后继的速度`。

每个节点还是两个指针域，如果节点有左孩子，则 lchild 指向左孩子，否则 lchild 指向其前驱；

如果节点有右孩子，则 rchild 指向右孩子，否则 rchild 指向其后继。

为了区分存储的是孩子节点还是前驱后继信息，我们增加两个标志域 ltag 和 rtag，节点的结构体如图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fda0e3314c53446b86fded743699e309.png)
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/209197d864c54fb589e4b310344611fa.png)
                                                                         
这种带有标志域的二叉链表称为`线索链表`；

指向前驱和后继的指针称为`线索`；

带有线索的二叉树称为`线索二叉树`；

以某种遍历方式将二叉树转化为线索二叉树的过程称为`线索化`。

## 构造线索二叉树

线索化实质是利用二叉链表中的空指针记录节点的前驱或后继线索。

每种遍历顺序的不同，节点的前驱和后继也不同。

因此二叉树线索化必须指明是什么遍历顺序的线索化。

线索二叉树分为前序线索二叉树、中序线索二叉树和后续线索二叉树。

`二叉树线索化的过程，实际上是在遍历过程中修改空指针的过程。`

可以设置两个指针，一个指针 pre 指向刚刚访问的节点，另一个指针 p 指向当前节点。

pre 指向的节点为 p 指向的节点的前驱；p 指向的节点为 pre 指向的节点的后继。

`在遍历的过程中，如果当前节点 p 的左孩子为空，则该节点的 lchild 指向其前驱，即 p.lchild = pre；如果 pre 节点的右孩子为空，则该节点的 rchild 指向其后继，即 pre.rchild = p。`

![在这里插入图片描述](https://img-blog.csdnimg.cn/fda0e3314c53446b86fded743699e309.png)


<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct Bnode {
    char data;
    Bnode *lchild, *rchild;
    bool ltag, rtag;
} *Btree;
```

<font color=#999AAA >java代码如下（示例）：

```java
private static class BNode {
    String data;
    BNode lchild, rchild;
    boolean ltag, rtag;
}
```



### 1). 算法步骤
在已有二叉树的前提下，线索化二叉树。

（中序线索化）
1. 指针 p 指向根节点，pre 初始化为空，pre 永远指向 p 的前驱。
2. 若 p 非空，则冲入一下操作：
                                                                         
	* 中序遍历线索化 p 的左子树。（递归）
                                                                         
	* 若 p 的左子树为空，则给 p 加上左线索，即 p.ltag = true，p 的左子树指针指向 pre(前驱)，即 p.lchild = pre；否则令 p.ltag = false。
	
	* 若 pre非空，则判断如果 pre 的右子树为空，给 pre 加上右线索，即 pre.rtag = true，pre 的右孩子指针指向 p (后继)，即 pre.rchild = p；否则令 pre.rtag = false。

	* p 赋值给 pre，p 转向 p 的右子树。
	
	* 中序遍历线索化 p 的右子树。（递归）
                                                                         
3. 处理最后一个节点，令其后继为空，即 pre.rchild = NULL；pre.rtag = true。


### 2). 图解
                                                                         
<font color=#CC3300>**以此二叉树为例：**</font>

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b5db6d4af864212a4129f4da3b87481.png)


（我。。我。。我。。我犯了个错误，我把 G 节点写成 E 了，要改太麻烦了。。凑活看吧。。。）
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/dea7570fc7334e52955c5e2f8f9e93d4.gif)


### 3).  代码
                                                                         
<font color=#999AAA >c++代码如下（示例）：

```cpp
void InThread(Btree &T) {
    Btree p = T;
    if (p) {
        InThread(p->lchild);
        if (p->lchild) {
            p->ltag = false;
        } else {
            p->ltag = true;
            p->lchild = pre;
        }

        if (pre) {
            if (pre->rchild) {
                pre->rtag = false;
            } else {
                pre->rtag = true;
                pre->rchild = p;
            }
        }

        pre = p;
        InThread(p->rchild);
    }
}

void CreateInThread(Btree &T) {
    pre = nullptr;
    if (T) {
        InThread(T);
        pre->rtag = true;
        pre->rchild = nullptr;
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void inThread(BNode t) {
	BNode p = t;
    if (p != null) {
        inThread(p.lchild);

        if (p.lchild != null) {
            p.ltag = false;
        } else {
            p.ltag = true;
            p.lchild = pre;
        }
        if (pre != null) {
            if (pre.rchild != null) {
                pre.rtag = false;
            } else {
                pre.rtag = true;
                pre.rchild = p;
            }
        }

        pre = p;
        inThread(p.rchild);
    }
}

public static void createInThread(BNode t) {
    pre = null;
    if (t != null) {
        inThread(t);
        pre.rtag = true;
        pre.rchild = null;
    }
}
```



##  遍历线索二叉树
                                                                         
### 1). 算法步骤

<font color=#CC3300>算法步骤：</font>
1. 指针 p 指向根节点。
2. 若 p 非空，则重复一下操作：
	* p 指针沿左孩子向下，找到最左节点，它是中序遍历的第一个节点；
	* 访问 p 节点；
	* 沿着右线索查找当前节点 p 的后继节点并访问，直到右线索为 `false` 或`遍历结束(pre->rchild = nullprt)`。
3. 遍历 p 的右子树。


### 2). 图解
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/5cb6282f37e8417da63d85ee7c0b1931.png)
                                                                         
$$
遍历顺序：DBEAFGC（脑补一下动图？）
$$

### 3). 代码
                                                                         
<font color=#999AAA >c++代码如下（示例）：

```cpp
void InOrderThread(Btree T) {
    Btree p = T;
    while (p) {
        while (!p->ltag) {
            p = p->lchild;
        }
        cout << p->data << " ";
        while (p->rtag && p->rchild) {
            p = p->rchild;
            cout << p->data << " ";
        }
        p = p->rchild;
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void inOrderThread(BNode t) {
    BNode p = t;
	while (p != null) {
        while (!p.ltag) {
            p = p.lchild;
        }
        System.out.print(p.data + " ");
        while (p.rtag && p.rchild != null) {
            p = p.rchild;
            System.out.print(p.data + " ");
        }
        p = p.rchild;
    }
}
```


## 完整代码
                                         
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>

using namespace std;

typedef struct Bnode {
    char data;
    Bnode *lchild, *rchild;
    bool ltag, rtag;
} *Btree;
Btree pre;

void CreateTree(Btree &T) {
    char ch;
    cin >> ch;
    if (ch != '#') {
        T = new Bnode;
        T->data = ch;
        CreateTree(T->lchild);
        CreateTree(T->rchild);
    } else {
        T = nullptr;
    }
}

void InThread(Btree &T) {
    Btree p = T;
    if (p) {
        InThread(p->lchild);
        if (p->lchild) {
            p->ltag = false;
        } else {
            p->ltag = true;
            p->lchild = pre;
        }

        if (pre) {
            if (pre->rchild) {
                pre->rtag = false;
            } else {
                pre->rtag = true;
                pre->rchild = p;
            }
        }

        pre = p;
        InThread(p->rchild);
    }
}

void CreateInThread(Btree &T) {
    pre = nullptr;
    if (T) {
        InThread(T);
        pre->rtag = true;
        pre->rchild = nullptr;
    }
}

void InOrderThread(Btree T) {
    Btree p = T;
    while (p) {
        while (!p->ltag) {
            p = p->lchild;
        }
        cout << p->data << " ";
        while (p->rtag && p->rchild) {
            p = p->rchild;
            cout << p->data << " ";
        }
        p = p->rchild;
    }
    cout << endl;
}

int main() {
    Btree T;
    CreateTree(T);
    CreateInThread(T);
    InOrderThread(T);
    return 0;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);

    private static class BNode {
        String data;
        BNode lchild, rchild;
        boolean ltag, rtag;
    }

    private static BNode pre;

    private static BNode createTree(BNode t) {
        String ch = sc.nextLine();
        if (!ch.equals("#")) {
            t = new BNode();
            t.data = ch;
            t.lchild = createTree(t.lchild);
            t.rchild = createTree(t.rchild);
        } else {
            t = null;
        }
        return t;
    }

    public static void inThread(BNode t) {
        BNode p = t;
        if (p != null) {
            inThread(p.lchild);

            if (p.lchild != null) {
                p.ltag = false;
            } else {
                p.ltag = true;
                p.lchild = pre;
            }
            if (pre != null) {
                if (pre.rchild != null) {
                    pre.rtag = false;
                } else {
                    pre.rtag = true;
                    pre.rchild = p;
                }
            }

            pre = p;
            inThread(p.rchild);
        }
    }

    public static void createInThread(BNode t) {
        pre = null;
        if (t != null) {
            inThread(t);
            pre.rtag = true;
            pre.rchild = null;
        }
    }

    public static void inOrderThread(BNode t) {
        BNode p = t;
        while (p != null) {
            while (!p.ltag) {
                p = p.lchild;
            }
            System.out.print(p.data + " ");
            while (p.rtag && p.rchild != null) {
                p = p.rchild;
                System.out.print(p.data + " ");
            }
            p = p.rchild;
        }
    }

    public static void main(String[] args) {
        BNode mytree = new BNode();
        mytree = createTree(mytree);
        System.out.println();
        createInThread(mytree);
        inOrderThread(mytree);
        System.out.println();

    }
}

```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结

`对于频繁查找前驱和后继的运算，线索二叉树优于普通二叉树`。

但是对于插入和删除操作，线索二叉树比普通二叉树开销大，因为除插入和删除操作外，还要修改相应的线索。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>树和森林的遍历
