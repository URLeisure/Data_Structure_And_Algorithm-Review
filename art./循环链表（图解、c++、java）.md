
<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 什么是循环链表？

>单链表中，只能向后，不能向前。
>如果从当前节点开始，无法访问该节点前面的节点，而最后一个节点的指针指向头节点，形成一个环，就可以从任何一个位置出发，访问所有的节点，这就是循环链表。

<font color=#CC3300>循环链表和普通链表的区别就是最后一个节点的后继指针指向了头节点。</font>

## 循环链表的存储方式（图解）
<font color=#CC3300>**单向循环链表**</font>的最后一个节点的next域不能为空，而是指向头节点。
                                                                         
![单向循环链表](https://img-blog.csdnimg.cn/616a692ae4a44080b9b181849b7f0fef.png)
                                                                         
<font color=#CC3300>**双向循环链表**</font>除了让最后一个节点的后继指针指向第一个节点外，还要让头节点的前驱指向最后一个节点。
                                                                         
![双向循环链表](https://img-blog.csdnimg.cn/0fb145d2e98841f6a2ab23a68288c3ad.png)


## 循环链表的基本操作
<font color=#999AAA >因为循环链表的取值、查找代码没有改变，这里就不再写了。

首先定义一个结构体（内部类），包含数据域与指针域。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct LNode{
    int data;
    LNode *next;
}*LinkList;
```

<font color=#999AAA >java代码如下（示例）：

```java
public static class LNode{
 	int date;
	LNode next;
}
```

### 1.初始化
循环链表的初始化是指构建一个空表。先创建一个头节点，不存储数据，然后令其指针域指向自己。

<font color=#CC3300>**单向循环链表**</font>

![单](https://img-blog.csdnimg.cn/b5398164436d4339ae2925f7dcffc4a6.png)

<font color=#CC3300>**双向循环链表**</font>
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/3d322d50ab2948cc9a99301c39c46cbb.png)

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Init(LinkList &L){
    L = new LNode;
    if(!L){
        return false;
    }
    L->next = L;
    L->data = 0;
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static LNode init(LNode L){
	LNode lNode = new LNode();
    lNode.next = lNode;
    lNode.data = 0;
    return lNode;
}
```

### 2.创建
循环链表的创建过程与单链表，双链表相同。

我们以头插法为例。
                                                                         
![头插法](https://img-blog.csdnimg.cn/aa1dc3d0cd9647529ef202c6fcdd2d9f.png)

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Create_H(LinkList &L,int n){
    LinkList s;
    while(n--){
        s = new LNode;
        cin>>s->date;
        //新节点next替换头节点next
        s->next = L->next;
        L->next = s;
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static void create_H(LNode L,int n){
    LNode s;
    Scanner sc = new Scanner(System.in);
        while(n-- > 0){
        s = new LNode();
        s.data = sc.nextInt();
        s.next = L.next;
        L.next = s;
    }
}
```



### 3.插入
插入也是相同的，只是在插入到最后一个节点时判断改一下。
                                                                         
![插入](https://img-blog.csdnimg.cn/33637d5498784c7d8adec40fd7f07323.png)


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Insert(LinkList &L,int i,int e){
    LinkList s,p = L;
    int j = 0;
    while(p->next!=L && j<i-1){
        p = p->next;
        j++;
    }
    if(j != i-1){
        return false;
    }
    s = new LNode;
    s->data = e;
    s->next = p->next;
    p->next = s;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static boolean insert(LNode L,int i,int e){
    LNode s,p = L;
    int j = 0;
    while(p.next!=L && j<i-1){
        p = p.next;
        j++;
    }
    if(j != i-1){
        return false;
    }
    s = new LNode();
    s.data = e;
    s.next = p.next;
    p.next = s;
    return true;
}
```

### 4.删除
删除也是相同的，只是在删除最后一个节点时判断改一下。

![删除](https://img-blog.csdnimg.cn/162f85f64f72463295fd1f5d43b2523b.png)

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Delete(LinkList &L,int i,int &e){
    LinkList q,p = L;
    int j = 0;
    while(p->next!=L && j<i-1){
        p = p->next;
        j++;
    }
    if(j != i-1){
        return false;
    }
    q = p->next;
    p->next = q->next;
    e = q->data;
    delete q;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int delete(LNode L,int i){
    LNode q,p = L;
    int j = 0;
    while(p.next!=L && j<i-1){
        p = p.next;
        j++;
    }
    if(j != j-1){
        return -1;
    }
    q = p.next;
    p.next = q.next;
    return q.data;
}
```

### 5.打印
打印非常简单，直接上代码。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Print(LinkList L){
    LinkList p = L->next;
    while(p != L){
        cout<<p->data<<" ";
        p = p->next;
    }
    cout<<endl;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static void print(LNode L){
    LNode p = L.next;
    while(p!= L){
        System.out.print(p.data + " ");
        p = p.next;
    }
    System.out.println();
}
```

### 6.释放内存
c++需要手动释放内存，而java不需要。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Destroy(LinkList &L){
    LinkList tmp;
    for(LinkList cur = L->next;cur!=NULL;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}
```

### 7.完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;

typedef struct LNode{
    int data;
    LNode *next;
}*LinkList;

bool Init(LinkList &L){
    L = new LNode;
    if(!L){
        return false;
    }
    L->next = L;
    L->data = 0;
    return true;
}

void Create_H(LinkList &L,int n){
    LinkList s;
    while(n--){
        s = new LNode;
        cin>>s->data;
        s->next = L->next;
        L->next = s;
    }
}

bool Insert(LinkList &L,int i,int e){
    LinkList s,p = L;
    int j = 0;
    while(p->next!=L && j<i-1){
        p = p->next;
        j++;
    }
    if(j != i-1){
        return false;
    }
    s = new LNode;
    s->data = e;
    s->next = p->next;
    p->next = s;
    return true;
}

bool Delete(LinkList &L,int i,int &e){
    LinkList q,p = L;
    int j = 0;
    while(p->next!=L && j<i-1){
        p = p->next;
        j++;
    }
    if(j != i-1){
        return false;
    }
    q = p->next;
    p->next = q->next;
    e = q->data;
    delete q;
    return true;
}

void Print(LinkList L){
    LinkList p = L->next;
    while(p != L){
        cout<<p->data<<" ";
        p = p->next;
    }
    cout<<endl;
}

void Destroy(LinkList &L){
    LinkList tmp;
    for(LinkList cur = L->next;cur!=L;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}
int main(){
    LinkList L;
    int choose=-1;
    int i,e,x,n;
    if(Init(L)){
        cout<<"初始化成功！"<<endl;
    }else{
        cout<<"初始化失败，请检查代码！"<<endl;
        return 0;
    }
    cout<<"输入元素个数："<<endl;
    cin>>n;
    cout<<"依次输入要插入的元素"<<endl;
    Create_H(L,n);
    cout << "双向链表表创建成功！"<<endl;

    cout << "1 .插入\n";
    cout << "2 .删除\n";
    cout << "3 .输出\n";
    cout << "0 .退出\n";
    while(choose != 0){
        cout<<"请选择！"<<endl;
        cin>>choose;
        switch (choose) {
            case 1:
                cout << "请输入要插入的位置和要插入的元素"<<endl;
                cin>>i>>e;
                if(Insert(L,i,e)){
                    cout << "插入成功！"<<endl;
                }else{
                    cout << "插入失败！"<<endl;
                }
                break;
            case 2:
                cout << "请输入要删除的位置i"<<endl;
                cin>>i;
                if(Delete(L,i,e)){
                    cout << "成功删除"<<e<<endl;
                }else{
                    cout<<"删除失败！"<<endl;
                }
                break;
            case 3:
                Print(L);
                break;
            case 0:
                cout << "成功退出"<<endl;
                break;
        }
    }
    Destroy(L);
    return 0;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;
public class E {

    public static class LNode{
        int data;
        LNode next;
    }

    public static LNode init(LNode L){
        LNode lNode = new LNode();
        lNode.next = lNode;
        lNode.data = 0;
        return lNode;
    }

    public static void create_H(LNode L,int n){
        LNode s;
        Scanner sc = new Scanner(System.in);
        while(n-- > 0){
            s = new LNode();
            s.data = sc.nextInt();
            s.next = L.next;
            L.next = s;
        }
    }

    public static boolean insert(LNode L,int i,int e){
        LNode s,p = L;
        int j = 0;
        while(p.next!=L && j<i-1){
            p = p.next;
            j++;
        }
        if(j != i-1){
            return false;
        }
        s = new LNode();
        s.data = e;
        s.next = p.next;
        p.next = s;
        return true;
    }

    public static int delete(LNode L,int i){
        LNode q,p = L;
        int j = 0;
        while(p.next!=L && j<i-1){
            p = p.next;
            j++;
        }
        if(j != j-1){
            return -1;
        }
        q = p.next;
        p.next = q.next;
        return q.data;
    }

    public static void print(LNode L){
        LNode p = L.next;
        while(p!= L){
            System.out.print(p.data + " ");
            p = p.next;
        }
        System.out.println();
    }
    public static void main(String[] args) {
        E e = new E();
        LNode L = new LNode();
        int n,x,i;
        Scanner sc = new Scanner(System.in);
        L = init(L);
        System.out.println("循环链表初始化成功！");


        System.out.println("输入元素个数：");
        n = sc.nextInt();
        System.out.println("依次输入要插入的元素");
        create_H(L,n);
        System.out.println("创建成功！");
        System.out.print("1.插入\n2.删除\n3.打印\n0.退出\n");
        int choose = -1;
        while(choose != 0){
            System.out.println("请输入");
            choose = sc.nextInt();
            switch(choose){
                case 1:
                    System.out.println("请输入要插入的位置和元素");
                    i = sc.nextInt();
                    x = sc.nextInt();
                    if(insert(L,i,x)){
                        System.out.println("插入成功！");
                    }else
                        System.out.println("插入失败！");
                    break;
                case 2:
                    System.out.println("请输入要删除的位置");
                    i = sc.nextInt();
                    System.out.println("已删除"+delete(L,i));
                    break;
                case 3:
                    print(L);
                    break;
                case 0:
                    break;
            }
        }
    }
}
```
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

#  链表的优缺点
**优点：**
1. 链表是动态存储，不需要预先分配最大空间
2. 插入删除不需要移动元素

**缺点：**

每次动态分配一个节点，每个节点的地址是不连续的，需要有指针域记录下一个节点的地址，<font color=#CC3300>**指针域需要占用一个 int 的空间，因此存储密度低**</font>（数据所占空间 ÷ 节点所占总空间）
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
                                                                         
双向循环链表和单向循环链表的改发基本相同，所以只发了单向循环链表的代码。

没想着偷懒，只是想着多出的时间赶紧复习下面的内容！！

通过观察我写的循环链表代码，总感觉和单链表没什么区别，循环查找好像没有什么用。其实用处还是蛮大的，只是受限于我的操作要求看起来没什么变化，在做相关题目的时候会发现，区别还是很大的。（废话）

<font color=#999AAA >下期预告：</font>栈链
