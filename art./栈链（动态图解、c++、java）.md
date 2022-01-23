

<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## 栈链概述（图解）

栈可以用顺序存储，也可以用链式存储，分别称为<font color=#CC3300>**顺序栈**</font>和<font color=#CC3300>**链栈**</font>。
                                                                         
![栈](https://img-blog.csdnimg.cn/13377058dc6048ff8129ffaafefe54b2.png)

<font color=#CC3300>**顺序栈是分配一段连续的空间**</font>，需要两个指针，base指向栈底，top指向栈顶。

<font color=#CC3300>**链栈每个节点的地址是不连续的**</font>，只需要一个栈顶指针即可。

从图中可以看出，链栈的每个节点都包含两个域：<font color=#CC3300>**数据域**</font>和<font color=#CC3300>**指针域**</font>。

## 链栈的基本操作
可以把链栈看作一个不带头节点的单链表，但只能在头部进行插入、删除。取值等操作，不可以在中间和尾部操作。因此，可以按单链表的方法定义链栈的节点。

首先定义一个结构体（内部类），包含一个数据域和一个指针域。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct SNode{
    int data;//数据域
    SNode *next;//指针域
}*LinkStack;
```

<font color=#999AAA >java代码如下（示例）：

```java
public class SNode{
    int data;
    SNode next;
}
```

### 1.初始化
初始化一个空的链栈是不需要头结点的，因此只需要让栈顶指针为空即可。


<font color=#999AAA >c++代码如下（示例）：

```cpp
void Init(LinkStack &S){
    S = NULL;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
F(){//直接用构造器初始化了，累了，毁灭吧
    s = null;
}
```

### 2.入栈
入栈是将新元素节点压入栈顶。因为链栈中第一个节点为栈顶，因此将新元素插入到第一个节点的前面，然后修改栈顶指针指向新节点即可。

![入栈](https://img-blog.csdnimg.cn/236a27f168dc48858b583296b23cba21.gif)


<font color=#999AAA >c++代码如下（示例）：

```cpp
void Push(LinkStack &S,int e){
    LinkStack p = new SNode;
    p->data = e;
    p->next = S;//新元素的下个地址是老的栈顶
    S = p;//栈顶上移
}
```

<font color=#999AAA >java代码如下（示例）：
```java
public void push(int e){
    SNode p = new SNode();
    p.data = e;
    p.next = s;
    s = p;
}
```

### 3.出栈
出栈就是把栈顶元素删除，让栈顶指针指向下一个节点，然后释放该节点空间（c++）。


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Pop(LinkStack &S,int &e){
    if(S == NULL){//栈空
        return false;
    }
    LinkStack p = S;//临时节点
    S = p->next;//栈顶下移
    e = p->data;//记录出栈元素
    delete p;//释放空间
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：
```java
public int pop(){
    if(s == null){
        return -1;
    }
    SNode p = s;
    s = s.next;
    return p.data;
}
```

### 4.取栈顶
取栈顶和出栈不同。

取栈顶元素只是吧栈顶元素复制一份，栈顶指针未移动，栈内元素个数未变。

<font color=#999AAA >c++代码如下（示例）：
```cpp
int GetTop(LinkStack S){
    if(S == NULL){
        return -1;
    }
    return S->data;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int getTop(){
    if(s == null){
        return -1;
    }
    return s.data;
}
```

### 5.完整代码
<font color=#999AAA >c++代码如下（示例）：
```cpp
#include<iostream>
using namespace std;

typedef struct SNode{
    int data;
    SNode *next;
}*LinkStack;

void Init(LinkStack &S){
    S = NULL;
}

void Push(LinkStack &S,int e){
    LinkStack p = new SNode;
    p->data = e;
    p->next = S;
    S = p;
}

bool Pop(LinkStack &S,int &e){
    if(S == NULL){
        return false;
    }
    LinkStack p = S;
    S = p->next;
    e = p->data;
    delete p;
    return true;
}

int GetTop(LinkStack S){
    if(S == NULL){
        return -1;
    }
    return S->data;
}

int main(){
    LinkStack S;
    int n,e;
    Init(S);
    cout<<"链栈初始化成功"<<endl;
    cout<<"输入元素个数："<<endl;
    cin>>n;
    cout<<"输入元素，依次入栈"<<endl;
    while(n--){
        cin>>e;
        Push(S,e);
    }
    cout<<"元素依次出栈！"<<endl;
    while(S!=NULL){
        cout<<GetTop(S)<<" ";
        Pop(S,e);
    }
    cout<<endl;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public class F {
    private SNode s;
    public class SNode{
        int data;
        SNode next;
    }

    F(){
        s = null;
    }

    public void push(int e){
        SNode p = new SNode();
        p.data = e;
        p.next = s;
        s = p;
    }

    public int pop(){
        if(s == null){
            return -1;
        }
        SNode p = s;
        s = s.next;
        return p.data;
    }

    public int getTop(){
        if(s == null){
            return -1;
        }
        return s.data;
    }

    public static void main(String[] args) {
        F f = new F();
        System.out.println("链栈初始化成功！");
        f.push(5);
        f.push(4);
        f.push(3);
        f.push(2);
        f.push(1);
        System.out.println("创建成功");
        System.out.println("元素依次出栈");
        while(f.s!=null){
            System.out.print(f.getTop()+" ");
            f.pop();
        }
        System.out.println();
    }
}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结

1. 顺序栈和链栈的基本操作都只需要常数时间，所以在时间效率上难分伯仲。
2. 在空间效率方面，顺序栈需要预先分配固定长度的空间，有可能造成空间的浪费或溢出
3. 链栈每次只分配一个节点，除非没有内存，否则不会出现溢出，但是每个节点需要一个指针域，结构性开销增加。

因此，如果元素个数变化较大，可以采用链栈；反之，可以采用顺序栈。

在实际应用中，顺序栈比链栈应用更广泛。


<font color=#999AAA >下期预告：</font> 循环队列
