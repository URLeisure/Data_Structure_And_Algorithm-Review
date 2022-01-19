

<font color=#999AAA >提示：以下是本篇文章正文内容，下面案例可供参考

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



## 什么是队列？

>队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。（百度百科）
>>这种<font color=#CC3300>**先进先出（First In First Out,FIFO）**</font>的线性序列，称为“队列”。它的操作受限，只能在两端操作：一端进，一端出。
<font color=#CC3300>**进的一端称为队尾（rear），出的一端称为队头（front）。队列可以用顺序存储，也可以用链式存储。**</font>


## 顺序队列概述（图解）

队列犹如火车穿山洞，先进去的部分先出来。（图片来源：淘宝）
                                                                         
![火车穿山洞](https://img-blog.csdnimg.cn/d46b61e486dc4a3cad54d2374b3579ef.png)
                                                                         
<font color=#CC3300>**队列的顺序存储采用一段连续的空间存储数据元素，并用两个整形变量记录对头和队尾元素的下标。**
                                                                         

![顺序队列](https://img-blog.csdnimg.cn/d30006b794f8482f93cdd03daabff26c.png)



## 顺序队列的基本操作
首先定义一个结构体（内部类），包含基地址和头尾索引。

<font color=#999AAA >c++代码如下（示例）：

```cpp
struct SqQueue{
    int *base;//基地址
    int front,rear;//头索引 尾索引
};
```
<font color=#999AAA >java代码如下（示例）：

```java
private class SqQueue{
    int []base;
   	int front,rear;
}
```

### 1.初始化
顺序队列定义好了之后，还要先定义一个最大的分配空间，顺序结构都是如此，需要预先分配空间。

![空队](https://img-blog.csdnimg.cn/d81dfe3281534deea4ad6c794d00e67e.png)

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool InitQueue(SqQueue &Q){
    Q.base = new int[Maxsize];//分配空间
    if(!Q.base){//创建失败
        return false;
    }
    Q.front = Q.rear = 0;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public boolean initQueue(){
    q = new SqQueue();
    q.base = new int[maxsize];
    if(q.base == null){
        return false;
    }
    q.front = q.rear = 0;
    return true;
}
```

### 2.入队
元素放入队尾rear的位置，然后rear后移一位。
                                                                         
![入队动画](https://img-blog.csdnimg.cn/9ff4663247894a92a22a3b072419a815.gif)

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool EnQueue(SqQueue &Q,int e){
    if(Q.rear>Maxsize - 1){//队满
        return false;
    }
    Q.base[Q.rear++] = e;//Q.base[Q.rear] = e; Q.rear++;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public boolean enQueue(int e){
    if(q.rear > maxsize-1){
        return false;
    }
    q.base[q.rear++] = e;
    return true;
}
```


### 2.出队
元素出队，然后队头front后移一位。

（在总结时会说明为何front后移之后还是队满状态。）

![出队](https://img-blog.csdnimg.cn/c49d9465ec7c488693c812941bf5b3b2.gif)

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool DeQueue(SqQueue &Q,int &e){//e为出队元素值
    if(Q.front == Q.rear){//队空
        return false;
    }
    e = Q.base[Q.front++];//出队
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int deQueue(){
    if(q.rear == q.front){
        return -1;
    }
    return q.base[q.front++];
}
```

### 3.查看队头元素
没什么好说的，太简单了。注意判断是否有元素就行。

<font color=#999AAA >c++代码如下（示例）：

```cpp
int GetHead(SqQueue Q){
    if(Q.front != Q.rear){//队列非空
        return Q.base[Q.front];
    }
    return -1;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int getHead(){
    if(q.rear != q.front){
        return q.base[q.front];
    }
    return -1;
}
```

### 4.队列长度

<font color=#999AAA >c++代码如下（示例）：

```cpp
int QueueLength(SqQueue Q){
    return (Q.rear-Q.front);
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int queueLength(){
    return q.rear-q.front;
}
```

### 5.完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;
#define Maxsize 7

struct SqQueue{
    int *base;
    int front,rear;
};

bool InitQueue(SqQueue &Q){
    Q.base = new int[Maxsize];
    if(!Q.base){
        return false;
    }
    Q.front = Q.rear = 0;
    return true;
}

bool EnQueue(SqQueue &Q,int e){
    if(Q.rear>Maxsize - 1){
        return false;
    }
    Q.base[Q.rear++] = e;
    return true;
}

bool DeQueue(SqQueue &Q,int &e){
    if(Q.front == Q.rear){
        return false;
    }
    e = Q.base[Q.front++];
    return true;
}

int GetHead(SqQueue Q){
    if(Q.front != Q.rear){
        return Q.base[Q.front];
    }
    return -1;
}

int QueueLength(SqQueue Q){
    return (Q.rear-Q.front);
}

int main(){
    SqQueue Q;
    int n,e;
    if(InitQueue(Q)){
        cout<<"队列初始化成功！"<<endl;
    }else{
        cout<<"队列初始化失败！"<<endl;
        return 0;
    }
    cout<<"输入元素个数"<<endl;
    cin>>n;
    cout<<"依次输入元素入队"<<endl;
    while(n--){
        cin>>e;
        EnQueue(Q,e);
    }
    cout<<"队列长度："<<QueueLength(Q)<<endl;
    cout<<"队头元素："<<GetHead(Q)<<endl;
    cout<<"开始出队："<<endl;
    while(DeQueue(Q,e)){
        cout<<"出队元素："<<e<<endl;
    }
    cout<<"队列长度："<<QueueLength(Q)<<endl;
    return 0;
}
```
<font color=#999AAA >java代码如下（示例）：
```java
public class B {
    private Scanner input = new Scanner(System.in);
    private final int maxsize = 7;
    private SqQueue q;
    private class SqQueue{
        int []base;
        int front,rear;
    }

    public boolean initQueue(){
        q = new SqQueue();
        q.base = new int[maxsize];
        if(q.base == null){
            return false;
        }
        q.front = q.rear = 0;
        return true;
    }

    public boolean enQueue(int e){
        if(q.rear > maxsize-1){
            return false;
        }
        q.base[q.rear++] = e;
        return true;
    }

    public int deQueue(){
        if(q.rear == q.front){
            return -1;
        }
        return q.base[q.front++];
    }

    public int getHead(){
        if(q.rear != q.front){
            return q.base[q.front];
        }
        return -1;
    }

    public int queueLength(){
        return q.rear-q.front;
    }

    public static void main(String[] args) {
        B b = new B();
        int n,x;
        if(b.initQueue()){
            System.out.println("顺序队列初始化成功！");
        }else{
            System.out.println("顺序队列初始化失败！");
        }
        System.out.println("输入元素个数");
        n = b.input.nextInt();
        System.out.println("请输入每个元素");
        while(n-- > 0){
            x = b.input.nextInt();
            b.enQueue(x);
        }

        System.out.println("队列元素个数:"+b.queueLength());
        System.out.println("队头元素是："+b.getHead());
        System.out.println("开始出队：");
        while((x=b.deQueue())!=-1){
            System.out.println("队头元素为："+x);
        }

        System.out.println("队列元素个数:"+b.queueLength());
    }

}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
从图解和代码不难看出，当rear指向Maxsize位置时，有元素出队，但当前还是队满状态。这种情况称为<font color=#CC3300>**“假溢出”**</font>。

<font color=#CC3300>**解决的方法也很简单，如图。需要用到简单的循环操作，在之后的文章中会有另一种队列——循环队列，来实现此方法。**</font>
  
![循环队列](https://img-blog.csdnimg.cn/ecde147be5ec406aa3fa1478dbb536c6.gif)

<font color=#999AAA >下期预告：</font> 顺序栈
