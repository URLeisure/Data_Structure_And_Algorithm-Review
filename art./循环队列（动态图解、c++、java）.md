
<font color=#999AAA >提示：以下是本篇文章正文内容，下面案例可供参考
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



## 循环队列概述（图解）

如图，模拟循环队列。
                                                                         
![循环队列](https://img-blog.csdnimg.cn/ecde147be5ec406aa3fa1478dbb536c6.gif)

1. <font color=#CC3300>**队空：**</font>无论队头和队尾在什么位置，只要 Q.rear 和 Q.front 指向同一个位置，就认为是队空。
2. <font color=#CC3300>**队满：**</font>当队尾 Q.rear 的下一个位置是 Q.front 时，就认为是队满。

<font color=#CC3300>**如何判断循环条件下 Q.rear 的下一个位置是 Q.front ？**

队列的最大空间设为 <font color=#CC3300>Maxsize</font> 。

想要在循环中判断，我们只要在 Q.rear 移动和运算时，添加取模运算（%）操作即可。

即队满条件为

>(Q.rear + 1) \% Maxsize = Q.front





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
public static class SqQueue{
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
public static boolean initQueue(SqQueue q){
    q.base = new int[maxsize];
    if(q.base == null){
        return false;
    }
    q.front = q.rear = 0;
    return true;
}
```

### 2.入队
判断队列是否已满。

元素放入队尾 Q.rear 所指空间，然后 Q.rear 后移一位。

Q.rear 后移时，判断是否属于临界条件，需要进行加1后的取模运算。
                                                                         
>Q.rear = (Q.rear + 1)\%Maxsize
                                                                         
![入队动画](https://img-blog.csdnimg.cn/84646abfb0724ad6ba1c380b188c3579.gif)
                                                                         
<font color=#999AAA >c++代码如下（示例）：
```cpp
bool EnQueue(SqQueue &Q,int e){
    if((Q.rear+1)%Maxsize == Q.front){//队满
        return false;
    }
    Q.base[Q.rear] = e;
    Q.rear = (Q.rear + 1)%Maxsize;//循环
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static boolean enQueue(SqQueue q,int e){
    if((q.rear+1)%maxsize == q.front){
        return false;
    }
    q.base[q.rear] = e;
    q.rear = (q.rear + 1)%maxsize;
    return true;
}
```


### 2.出队
判断队列是否为空。

元素出队，然后队头front后移一位。

Q.front 后移时，判断是否属于临界条件，需要进行加1后的取模运算。
                                                                         

>Q.front = (Q.front + 1)\%Maxsize

                                                                         
![出队](https://img-blog.csdnimg.cn/c31c53d46fc340ddaab93c5e152ebd50.gif)


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool DeQueue(SqQueue &Q,int &e){//出队元素e
    if(Q.rear == Q.front){//队空
        return false;
    }
    e = Q.base[Q.front];
    Q.front = (Q.front + 1)%Maxsize;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int deQueue(SqQueue q){
    if(q.rear == q.front){
        return -1;
    }
    int e = q.base[q.front];
    q.front = (q.front + 1)%maxsize;
    return e;
}
```

### 3.查看队头元素
注意判断队列是否有元素就行。

<font color=#999AAA >c++代码如下（示例）：

```cpp
int GetHead(SqQueue Q){
    if(Q.rear == Q.front){
        return -1;
    }
    return Q.base[Q.front];
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int getHead(SqQueue q){
    if(q.rear == q.front){
        return -1;
    }
    return q.base[q.front];
}
```

### 4.队列长度
因为是循环队列，判断队长时直接做差可能会有负数。需要经过取模运算处理。

>(Q.rear - Q.front + Maxsize)\%Maxsize

<font color=#999AAA >c++代码如下（示例）：

```cpp
int QueueLength(SqQueue Q){
    return (Q.rear - Q.front + Maxsize)%Maxsize;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public static int queueLength(SqQueue q){
    return (q.rear - q.front +maxsize)%maxsize;;
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
    if((Q.rear+1)%Maxsize == Q.front){
        return false;
    }
    Q.base[Q.rear] = e;
    Q.rear = (Q.rear + 1)%Maxsize;
    return true;
}

bool DeQueue(SqQueue &Q,int &e){
    if(Q.rear == Q.front){
        return false;
    }
    e = Q.base[Q.front];
    Q.front = (Q.front + 1)%Maxsize;
    return true;
}

int GetHead(SqQueue Q){
    if(Q.rear == Q.front){
        return -1;
    }
    return Q.base[Q.front];
}

int QueueLength(SqQueue Q){
    return (Q.rear - Q.front + Maxsize)%Maxsize;
}


int main(){
    SqQueue Q;
    int n,x;
    if(InitQueue(Q)){
        cout<<"循环队列初始化成功！"<<endl;
    }else{
        cout<<"循环队列初始化失败！"<<endl;
        return 0;
    }
    cout<<"请输入元素个数n："<<endl;
    cin>>n;
    if(n > Maxsize-1){
        cout<<"溢出"<<endl;
        return 0;
    }
    cout<<"请依次输入元素"<<endl;
    while(n--){
        cin>>x;
        EnQueue(Q,x);
    }
    cout<<endl;
    cout<<"队列元素个数"<<QueueLength(Q)<<endl;
    cout<<"对头元素："<<GetHead(Q)<<endl;
    cout<<"元素依次出队："<<endl;
    while(DeQueue(Q,x)){
        cout<<x<<" ";
    }
    cout<<endl;
    cout<<"队列元素个数"<<QueueLength(Q)<<endl;
}
```
<font color=#999AAA >java代码如下（示例）：
```java
import java.util.Scanner;
public class G {
    private static int maxsize = 7;
    public static class SqQueue{
        int []base;
        int front,rear;
    }

    public static boolean initQueue(SqQueue q){
        q.base = new int[maxsize];
        if(q.base == null){
            return false;
        }
        q.front = q.rear = 0;
        return true;
    }

    public static boolean enQueue(SqQueue q,int e){
        if((q.rear+1)%maxsize == q.front){
            return false;
        }
        q.base[q.rear] = e;
        q.rear = (q.rear + 1)%maxsize;
        return true;
    }

    public static int deQueue(SqQueue q){
        if(q.rear == q.front){
            return -1;
        }
        int e = q.base[q.front];
        q.front = (q.front + 1)%maxsize;
        return e;
    }

    public static int getHead(SqQueue q){
        if(q.rear == q.front){
            return -1;
        }
        return q.base[q.front];
    }

    public static int queueLength(SqQueue q){
        return (q.rear - q.front +maxsize)%maxsize;
    }

    public static void main(String[] args) {
        SqQueue q=new SqQueue();
        Scanner sc=new Scanner(System.in);
        int n,x;
        if(initQueue(q)){
            System.out.println("循环队列初始化成功！");
        }else{
            System.out.println("初始化失败！");
        }
        System.out.println("元素个数n：");
        n=sc.nextInt();
        System.out.println("请依次输入每个元素");
        while(n-- > 0){
            x=sc.nextInt();
            enQueue(q,x);
        }
        System.out.println("元素个数："+queueLength(q));
        System.out.println("循环队列头元素："+getHead(q));
        System.out.println("依次出队！");
        while(true){
            x=deQueue(q);
            if(x!=-1){
                System.out.println(x+" ");
            }else{
                break;
            }
        }
        System.out.println("元素个数："+queueLength(q));
    }
}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
以上便是今天的复习内容了。

<font color=#999AAA >下期预告：</font> 链队列
