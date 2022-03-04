<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 一、概述
                                                                         
* 假设以三元组（F，C，L / R）的形式输入一棵二叉树的诸边（`其中 F 是双亲节点的标识，C 是孩子节点表示，L / R 表示 C 为 F 的左孩子 / 右孩子`），且在输入的三元组序列中，C 是按层次顺序出现的。
* 设节点的标识是字符类型：
* `F 为 NULL 时：`
	* C 不为 NULL，C 为根节点标识；
	* C 为 NULL，表示输入结束。


## 二、算法步骤
因为是按层次输入，所及可以借助队列实现。

<font color=#CC3300>**算法步骤：**</font>

1. 输入第一组数据，创建根节点入队；
2. 输入下一组数据；
3. 如果队列非空且输入数据前两项非空，则队头元素出队；
4. 判断输入数据中的双亲是否和队头元素相等
	* 不相等，则转向第 3 步；
	* 相等，则创建一个新节点，判断该节点使其双亲的左孩子还是右孩子，并做相应的处理，然后新节点入队。
	* 输入下一组数据，转向第 4 步（因为一个对头元素可能有两个孩子，所以不能创建一个孩子就结束）。
5. 直到队列为空或者输入数据前为两项为空，算法停止。

## 三、图解
<font color=#CC3300>**输入如下：**</font>

```java
NULL A L
A B L
A C R
B D R
C E L
C F R
D G L
F H L
NULL NULL L
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/95e6f43a1c1144e2a4d563e26aac14c5.gif)


## 四、代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateTree(Btree &T) {
    string a, b, c;
    cin >> a >> b >> c;
    Btree p, node;
    queue<Btree> q;
    if (a == "NULL" && b != "NULL") {
        node = new BNode;
        node->data = b;
        node->lchild = node->rchild = nullptr;
        T = node;
        q.push(node);
    }
    cin >> a >> b >> c;
    while (!q.empty() && a != "NULL" && b != "NULL") {
        p = q.front();
        q.pop();
        while (p->data == a) {
            node = new BNode;
            node->data = b;
            node->rchild = node->lchild = nullptr;
            if (c == "L") {
                p->lchild = node;
            } else {
                p->rchild = node;
            }
            q.push(node);
            cin >> a >> b >> c;
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
private static BNode createTree(BNode t) {
    BNode p, node;
    String a = sc.next();
    String b = sc.next();
    String c = sc.next();
    LinkedList<BNode> q = new LinkedList<>();
  	if (a.equals("NULL") && !b.equals("NULL")) {
        node = new BNode();
        node.data = b;
        node.lchild = node.rchild = null;
        t = node;
        q.add(node);
    }
    a = sc.next();
    b = sc.next();
    c = sc.next();
  	while (!q.isEmpty() && !a.equals("NULL") && !b.equals("NULL")) {
      	p = q.pop();
        while (p.data.equals(a)) {
            node = new BNode();
            node.data = b;
            node.lchild = node.rchild = null;
            if (c.equals("L")) {
                p.lchild = node;
            } else {
                p.rchild = node;
            }
            q.add(node);
            a = sc.next();
            b = sc.next();
            c = sc.next();
        }
    }
    return t;
}
```

## 五、完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include<queue>

using namespace std;

typedef struct BNode {
    string data;
    BNode *rchild, *lchild;
} *Btree;

void CreateTree(Btree &T) {
    string a, b, c;
    cin >> a >> b >> c;
    Btree p, node;
    queue<Btree> q;
    if (a == "NULL" && b != "NULL") {
        node = new BNode;
        node->data = b;
        node->lchild = node->rchild = nullptr;
        T = node;
        q.push(node);
    }
    cin >> a >> b >> c;
    while (!q.empty() && a != "NULL" && b != "NULL") {
        p = q.front();
        q.pop();
        while (p->data == a) {
            node = new BNode;
            node->data = b;
            node->rchild = node->lchild = nullptr;
            if (c == "L") {
                p->lchild = node;
            } else {
                p->rchild = node;
            }
            q.push(node);
            cin >> a >> b >> c;
        }
    }
}

void preOrder(Btree T) {
    if (T) {
        cout << T->data << " ";
        preOrder(T->lchild);
        preOrder(T->rchild);
    }
}

int main() {
    Btree t;
    CreateTree(t);
    preOrder(t);
    cout << endl;
}
/*
NULL A L
A B L
A C R
B D R
C E L
C F R
D G L
F H L
NULL NULL L
*/
```

<font color=#999AAA >java代码如下（示例）：

```java
import javax.security.auth.login.CredentialException;
import java.util.LinkedList;
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);

    private static class BNode {
        String data;
        BNode lchild, rchild;
    }

    private static BNode createTree(BNode t) {
        BNode p, node;
        String a = sc.next();
        String b = sc.next();
        String c = sc.next();
        LinkedList<BNode> q = new LinkedList<>();
        if (a.equals("NULL") && !b.equals("NULL")) {
            node = new BNode();
            node.data = b;
            node.lchild = node.rchild = null;
            t = node;
            q.add(node);
        }
        a = sc.next();
        b = sc.next();
        c = sc.next();
        while (!q.isEmpty() && !a.equals("NULL") && !b.equals("NULL")) {
            p = q.pop();
            while (p.data.equals(a)) {
                node = new BNode();
                node.data = b;
                node.lchild = node.rchild = null;
                if (c.equals("L")) {
                    p.lchild = node;
                } else {
                    p.rchild = node;
                }
                q.add(node);
                a = sc.next();
                b = sc.next();
                c = sc.next();
            }
        }
        return t;
    }

    private static void preOrder(BNode t) {
        if (t != null) {
            System.out.print(t.data + " ");
            preOrder(t.lchild);
            preOrder(t.rchild);
        }
    }

    public static void main(String[] args) {
        BNode t = new BNode();
        t = createTree(t);
        preOrder(t);
    }
}
/*
NULL A L
A B L
A C R
B D R
C E L
C F R
D G L
F H L
NULL NULL L
*/
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>还原二叉树
