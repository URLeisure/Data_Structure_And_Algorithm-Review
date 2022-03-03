
## 二叉树的深度

### 1). 算法步骤

<font color=#CC3300>**算法步骤：**</font>

1. 如果二叉树为空，则深度为 0；
2. 非空则深度为根的左、右子树的深度最大值加 1。

### 2). 代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
int Depth(Btree T) {
    int m = 0, n = 0;
    if (!T) {
        return 0;
    }
    m = Depth(T->lchild);
    n = Depth(T->rchild);
    if (m > n) {
        return m + 1;
    }
    return n + 1;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static int depth(BNode t) {
    int m = 0, n = 0;
    if (t == null) {
        return 0;
    }
    m = depth(t.lchild);
    n = depth(t.rchild);
    if (m > n) {
        return m + 1;
    }
    return n + 1;
}
```

## 二叉树的叶子数
  
特殊情况：如果二叉树为空，则叶子数为 0；如果根的左、右子树都为空，则叶子数为 1；一般情况下`二叉树的叶子数等于左子树的叶子数与右子树的叶子数之和`。
  
### 1). 算法步骤
  
<font color=#CC3300>**算法步骤：**</font>

1. 如果二叉树为空，则叶子数为 0；
2. 如果根的左、右子树都为空，则叶子数为 1；
3. 否则求左子树的叶子数和右子树的叶子数之和，即为二叉树的叶子数。

### 2). 代码
  
<font color=#999AAA >c++代码如下（示例）：

```cpp
int LeafCount(Btree T) {
    if (!T) {
        return 0;
    }
    if (!T->lchild && !T->rchild) {
        return 1;
    }
    return LeafCount(T->lchild) + LeafCount(T->rchild);
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static int leafCount(BNode t) {
    if (t == null) {
        return 0;
    }
    if (t.lchild == null && t.rchild == null) {
        return 1;
    }
    return leafCount(t.lchild) + leafCount(t.rchild);
}
```

## 二叉树的节点数
  
节点数很简单，全部节点过一遍就 ok 了。
  
### 1). 算法步骤
  
<font color=#CC3300>**算法步骤：**</font>
  
1. 如果当前节点为空，结束递归；
2. 如果不为空，递归其左、右子树，并加 1。

### 2). 代码
  
<font color=#999AAA >c++代码如下（示例）：

```cpp
int NodeCount(Btree T) {
    if (!T) {
        return 0;
    }
    return NodeCount(T->lchild) + NodeCount(T->rchild) + 1;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static int nodeCount(BNode t){
    if(t == null){
        return 0;
    }
    return nodeCount(t.lchild) + nodeCount(t.rchild) + 1;
}
```

## 整合代码
  
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>

using namespace std;

typedef struct BNode {
    char data;
    BNode *lchild, *rchild;
} *Btree;

void CreateTree(Btree &T) {
    char ch;
    cin >> ch;
    if (ch != '#') {
        T = new BNode;
        T->data = ch;
        CreateTree(T->lchild);
        CreateTree(T->rchild);
    } else {
        T = nullptr;
    }
}

int Depth(Btree T) {
    int m = 0, n = 0;
    if (!T) {
        return 0;
    }
    m = Depth(T->lchild);
    n = Depth(T->rchild);
    if (m > n) {
        return m + 1;
    }
    return n + 1;
}

int LeafCount(Btree T) {
    if (!T) {
        return 0;
    }
    if (!T->lchild && !T->rchild) {
        return 1;
    }
    return LeafCount(T->lchild) + LeafCount(T->rchild);
}

int NodeCount(Btree T) {
    if (!T) {
        return 0;
    }
    return NodeCount(T->lchild) + NodeCount(T->rchild) + 1;
}

int main() {
    Btree T;
    CreateTree(T);
    cout << Depth(T) << endl;
    cout << LeafCount(T) << endl;
    cout << NodeCount(T) << endl;
    return 0;
}
// ABD##E##CF#G###
```
  
<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    public static Scanner sc = new Scanner(System.in);
    
    public static class BNode {
        String data;
        BNode lchild, rchild;
    }

    public static BNode createTree(BNode t) {
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

    public static int depth(BNode t) {
        int m = 0, n = 0;
        if (t == null) {
            return 0;
        }
        m = depth(t.lchild);
        n = depth(t.rchild);
        if (m > n) {
            return m + 1;
        }
        return n + 1;
    }

    public static int leafCount(BNode t) {
        if (t == null) {
            return 0;
        }
        if (t.lchild == null && t.rchild == null) {
            return 1;
        }
        return leafCount(t.lchild) + leafCount(t.rchild);
    }

    public static int nodeCount(BNode t){
        if(t == null){
            return 0;
        }
        return nodeCount(t.lchild) + nodeCount(t.rchild) + 1;
    }
    
    public static void main(String[] args) {
        BNode t = new BNode();
        t = createTree(t);
        System.out.println(depth(t));
        System.out.println(leafCount(t));
        System.out.println(nodeCount(t));
    }
}
/*
A
B
D
#
#
E
#
#
C
F
#
g
#
#
#
 */
```

## 总结
  
今天的内容挺水的= =

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>三元组创建二叉树
