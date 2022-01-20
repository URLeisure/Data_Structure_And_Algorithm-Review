

<font color=#999AAA >提示：以下是本篇文章正文内容，下面案例可供参考

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



## 链队列概述（图解）
队列除了用顺序存储，也可以用链式存储。

两种存储方式如图
                                                                         
![顺序队列](https://img-blog.csdnimg.cn/d30006b794f8482f93cdd03daabff26c.png)

![链队列](https://img-blog.csdnimg.cn/8025f4934e52484ea9b131ad31b97d55.png)

顺序队列是分配一段连续的空间，用两个整形下标front和rear分别指向队头和队尾。

链队列类似一个单链表，需要两个指针front和rear分别指向队头和队尾。

从队头出队，从队尾入队，为了出队时删除元素方便，可以增加一个头节点。

<font color=#CC3300>**链队列需要头节点**</font>。


## 顺序队列的基本操作
因为链队列就是一个单链表的形式，因此可以借助单链表的定义。

<font color=#CC3300>**链队列的操作和单链表一样，只不过它只能队头删除，队尾插入，是操作受限的单链表**</font>。

首先定义两个结构体（内部类），一个包含数据域和指针域，另一个包含 front 和 rear 指针。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct QNode{//元素节点
    int data;
    QNode *next;
}*Qptr;

struct LinkQueue{
    QNode *front;
    QNode *rear;
};
```
<font color=#999AAA >java代码如下（示例）：

```java
public static class Node{//元素节点
    int data;
    Node next;
}
public static class Link{
    Node front;
    Node rear;
}
```

### 1.初始化
链队列的初始化，创建一个头节点，头指针和尾指针指向头节点。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void InitQueue(LinkQueue &Q){
    Q.front = Q.rear = new QNode;//指向头节点
    Q.front->next = nullptr;
};
```
<font color=#999AAA >java代码如下（示例）：

```java
public static void initQueue(Link q){
    q.front = q.rear = new Node();
    q.front.next = null;
}
```

### 2.入队
1. 先建立一个新节点，将元素存入该节点的数据域
2. 将新节点插入队尾，尾指针后移。

![入队](https://img-blog.csdnimg.cn/fc76dfcde0d34622afddb6529120c158.png)



<font color=#999AAA >c++代码如下（示例）：

```cpp
void EnQueue(LinkQueue &Q,int e){
    Qptr s = new QNode;
    s->data = e;
    s->next = nullptr;
    Q.rear->next = s;
    Q.rear = s;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static void addQueue(Link q,int e){
    Node s = new Node();
    s.data = e;
    s.next = null;
    q.rear.next = s;
    q.rear = s;
}
```


### 2.出队

出队即为删除第一个数据元素，将第一个数据节点跳过。

![出队](https://img-blog.csdnimg.cn/2d8196da7dce49f4a86de4d887d23608.png)


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool DeQueue(LinkQueue &Q,int &e){
    if(Q.front == Q.rear){
        return false;
    }
    Qptr p = Q.front->next;
    Q.front->next = p->next;
    e = p->data;
    if(p == Q.rear){
        Q.rear = Q.front;
    }
    delete p;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int deQueue(Link q){
    if(q.rear == q.front){
        return -1;
    }
    Node p = q.front.next;
    q.front.next = p.next;
    if(p == q.rear){
        q.rear = q.front;
    }
    return p.data;
}
```

### 3.查看队头元素


<font color=#999AAA >c++代码如下（示例）：

```cpp
int GetHead(LinkQueue Q){
    if(Q.rear == Q.front){
        return -1;
    }
    return Q.front->next->data;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int getHead(Link q){
    if(q.rear == q.front){
        return -1;
    }
    return q.front.next.data;
}
```

### 4.完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;

typedef struct QNode{
    int data;
    QNode *next;
}*Qptr;

struct LinkQueue{
    QNode *front;
    QNode *rear;
};

void InitQueue(LinkQueue &Q){
    Q.front = Q.rear = new QNode;
    Q.front->next = nullptr;
};

void EnQueue(LinkQueue &Q,int e){
    Qptr s = new QNode;
    s->data = e;
    s->next = nullptr;
    Q.rear->next = s;
    Q.rear = s;
}

bool DeQueue(LinkQueue &Q,int &e){
    if(Q.front == Q.rear){
        return false;
    }
    Qptr p = Q.front->next;
    Q.front->next = p->next;
    e = p->data;
    if(p == Q.rear){
        Q.rear = Q.front;
    }
    delete p;
    return true;
}

int GetHead(LinkQueue Q){
    if(Q.rear == Q.front){
        return -1;
    }
    return Q.front->next->data;
}

int main(){
    LinkQueue q;
    int n,x;
    InitQueue(q);
    cout<<"链队列初始化成功！"<<endl;
    cout<<"输入个数："<<endl;
    cin>>n;
    cout<<"依次输入"<<endl;
    while(n--){
        cin>>x;
        EnQueue(q,x);
    }
    cout<<"对头元素："<<GetHead(q)<<endl;
    cout<<"元素依次出队："<<endl;
    while(true){
        if(DeQueue(q,x)){
            cout<<x<<" ";
        }else{
            break;
        }
    }
}

```
<font color=#999AAA >java代码如下（示例）：
```java
import java.util.Scanner;
public class H {
    public static class Node{
        int data;
        Node next;
    }
    public static class Link{
        Node front;
        Node rear;
    }

    public static void initQueue(Link q){
        q.front = q.rear = new Node();
        q.front.next = null;
    }

    public static void addQueue(Link q,int e){
        Node s = new Node();
        s.data = e;
        s.next = null;
        q.rear.next = s;
        q.rear = s;
    }

    public static int deQueue(Link q){
        if(q.rear == q.front){
            return -1;
        }
        Node p = q.front.next;
        q.front.next = p.next;
        if(p == q.rear){
            q.rear = q.front;
        }
        return p.data;
    }

    public static int getHead(Link q){
        if(q.rear == q.front){
            return -1;
        }
        return q.front.next.data;
    }

    public static void main(String[] args) {
        Link q = new Link();
        initQueue(q);
        System.out.println("链队列初始化成功！");
        System.out.println("输入元素个数：");
        Scanner input = new Scanner(System.in);
        int n = input.nextInt();
        System.out.println("请输入每个元素");
        while(n-- > 0){
            int x = input.nextInt();
            addQueue(q,x);
        }

        System.out.println("队头元素是："+getHead(q));
        System.out.println("出队：");
        int e;
        while((e = deQueue(q)) != -1){
            System.out.println("出队元素为："+e);
        }
    }
}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
以上便是今天的复习内容，之后两天会涉及到字符串匹配的BF和KMP，比较难啃。

<font color=#999AAA >下期预告：</font>字符串模式匹配算法-BF算法
