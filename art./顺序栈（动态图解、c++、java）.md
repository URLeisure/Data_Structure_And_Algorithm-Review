
<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 什么是栈？
>栈（stack）又名堆栈，它是一种运算受限的线性表。限定仅在表尾进行插入和删除操作的线性表。这一端被称为栈顶，相对地，把另一端称为栈底。向一个栈插入新元素又称作进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。(百度百科)
>
>![请添加图片描述](https://img-blog.csdnimg.cn/93a1996a55334309ac59948fd333a55e.gif)
>                                                                        
>(我也不知道这是谁的图。。。。。图片来源：<font color=#CC3300>**知乎：不淡定的小淡定** </font> 侵删)
>>这种**后进先出（Last In First Out，LIFO）**的线性序列，称为“栈”。
>栈也是一种线性表，只不过它是操作受限的线性表，只能在一端进出操作。
>进的一端称为栈顶（top），另一端称为栈底（base）。
>><font color=#CC3300>**栈可以用顺序存储，也可以用链式存储，分别称为顺序栈和链栈**</font>。

## 顺序栈概述（图解）

![栈](https://img-blog.csdnimg.cn/0936767b591143a28c149149607ea6e6.png)
                                                                         
顺序栈需要两个指针，base指向栈底，top指向栈顶。

初始状态 top 指向 base(top -> base)。

## 顺序栈的基本操作

c++写法和java写法有一定的区别。

c++首先定义一个结构体，包含两个指针。

<font color=#999AAA >c++代码如下（示例）：

```cpp
struct SqStack{
    int *base;//栈底指针
    int *top;//栈顶指针
};
```

<font color=#999AAA >java代码如下（示例）：

```java
private int []date;
private int top;
```

### 1.初始化
栈定义好了之后，还要先定义一个最大的分配空间，顺序结构都是如此，需要预先分配空间。

初始化一个空栈，动态分配Maxsize大小的空间，用top和base指向该空间的基地址。
                                                                         
![空栈](https://img-blog.csdnimg.cn/c127e905e05e4e6bb13cd4ad4e772193.png)


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool InitStack(SqStack &S){
    S.base = new int[Maxsize];//分配空间
    if(!S.base){//分配失败
        return false;
    }
    S.top = S.base;//空栈
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public boolean init(){
	date = new int[maxsize];//分配空间
    if(date == null){//分配失败
        return false;
    }
    top = 0;//没有元素
    return true;
}
```

### 2.入栈
入栈前判断是否栈满，如果栈已满，则入栈失败。

否则将元素放入栈顶，栈顶指针上移一位（top++）。

<font color=#CC3300>**栈顶指针（top）指向栈顶元素的上一位。** </font>

![入栈](https://img-blog.csdnimg.cn/5a5f33c6d12d46f89a4be8ed9c093e8e.gif)



<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Push(SqStack &S,int e){//插入e为新的栈顶元素
    if(S.top-S.base == Maxsize){
        return false;
    }
    *S.top = e;
    S.top ++;//top指针上移
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：
```java
public boolean push(int e){//插入e为新的栈顶元素
    if(top == maxsize-1){
        return false;
    }
    date[top++] = e;//top指针上移
    return true;
}
```

### 3.出栈
出栈前判断是否栈空，如果栈是空的，则出栈失败；

否则栈顶指针向下移动一个位置（top--），讲栈顶元素暂存给一个变量。

<font color=#CC3300>**先将栈顶指针下移得到栈顶元素，再“删除”（直接忽视该元素，下次进栈会覆盖该元素）栈顶元素 。**</font>
                                                                         
![出栈](https://img-blog.csdnimg.cn/4fe9511c7ac94099b5863b0258bdf70c.gif)


<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Pop(SqStack &S,int &e){//暂存到变量e中
    if(S.top == S.base){//栈空
        return false;
    }
    e = *(--S.top);//栈顶指针--，将最新的栈顶元素付给e
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：
```java
public int pop(){
    if(top == 0){//栈空
        return -1;
    }
    return date[--top];
}
```

### 4.取栈顶
取栈顶和出栈不同。

取栈顶元素只是吧栈顶元素复制一份，栈顶指针未移动，栈内元素个数未变。

<font color=#999AAA >c++代码如下（示例）：
```cpp
int GetTop(SqStack S){
    if(S.top != S.base){//栈非空
        return *(S.top-1);
    }
    return -1;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int getTop(){
	if(top == 0){
      	return -1;
    }
    return date[top - 1];
}
```

### 5.完整代码
<font color=#999AAA >c++代码如下（示例）：
                                                                         
```cpp
#include<iostream>
using namespace std;
#define Maxsize 100

struct SqStack{
    int *base;
    int *top;
};

bool InitStack(SqStack &S){
    S.base = new int[Maxsize];
    if(!S.base){
        return false;
    }
    S.top = S.base;
    return true;
}

bool Push(SqStack &S,int e){
    if(S.top-S.base == Maxsize){
        return false;
    }
    *S.top = e;
    S.top ++;
    return true;
}

bool Pop(SqStack &S,int &e){
    if(S.top == S.base){
        return false;
    }
    e = *(--S.top);
    return true;
}

int GetTop(SqStack S){
    if(S.top != S.base){
        return *(S.top-1);
    }
    return -1;
}

int main(){
    SqStack S;
    int n,x;
    if(InitStack(S)){
        cout<<"初始化成功！"<<endl;
    }else{
        cout<<"初始化失败，请检查代码！"<<endl;
    }
    cout<<"请输入元素个数："<<endl;
    cin>>n;
    cout<<"依次输入："<<endl;
    while(n--){
        cin>>x;
        Push(S,x);
    }
    cout<<"依次出栈："<<endl;
    while(S.top != S.base){
        cout<<GetTop(S)<<endl;
        Pop(S,x);
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public class C {
    private final int maxsize = 100;
    private Scanner input = new Scanner(System.in);
    private int []date;
    private int top;

    public boolean init(){
        date = new int[maxsize];
        if(date == null){
            return false;
        }
        top = 0;
        return true;
    }

    public boolean push(int e){
        if(top == maxsize-1){
            return false;
        }
        date[top++] = e;
        return true;
    }

    public int pop(){
        if(top == 0){
            return -1;
        }
        return date[--top];
    }

    public int getTop(){
        if(top == 0){
            return -1;
        }
        return date[top - 1];
    }

    public static void main(String[] args) {
        int n,x;
        C c = new C();
        if(c.init()){
            System.out.println("顺序栈初始化成功！");
        }else{
            System.out.println("初始化失败，请检查代码！");
        }
        System.out.println("输入元素个数");
        n = c.input.nextInt();
        System.out.println("依次输入元素");

        while(n-- >0){
            x = c.input.nextInt();
            c.push(x);
        }
        System.out.println("依次出栈");
        while(c.top > 0){
            System.out.println("栈顶元素为"+c.getTop());
            System.out.println("出栈元素为"+c.pop());
        }
    }

}

```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结

记住羽毛球那个图，这就是栈hh。人为规定栈不允许在中间查找、取值、插入、删除等操作。

顺序栈本身是顺序存储的，如果偏要从中间取一个元素，也是可以实现的，但就不是栈了。



<font color=#999AAA >下期预告：</font> 双向链表
