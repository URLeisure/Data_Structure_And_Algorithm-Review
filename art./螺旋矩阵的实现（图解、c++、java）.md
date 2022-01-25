<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


<font color=#CC3300></font> 

## 什么是螺旋矩阵？

直接看图就能理解。

3阶螺旋矩阵：
                                                                         
![3阶螺旋矩阵](https://img-blog.csdnimg.cn/a095c755d078453f8fd8e0fee3310a30.png)
                                                                         
4阶螺旋矩阵：

![4阶螺旋矩阵](https://img-blog.csdnimg.cn/7a593b1fb9cd486fa768d80cd3fb6882.png)


螺旋矩阵的实现很简单，只是为了了解如何在二维数组里上下左右的移动，强化基础。


## 创建思路
创建方法很简单，别找规律一行一行输就行。。。。

如图，我们本次的螺旋矩阵按照右下左上的顺序完成创建。

![4阶螺旋矩阵](https://img-blog.csdnimg.cn/10bf7705e04f43d5b5dc6305ec96957f.png)
                                                                         
其实也没什么思路，就是先一直朝向一个方向创建，创建过程中判断是否需要转向，然后继续创建。

我们默认数组操作前数值全部为 0 ，而边界为 -1。

则判断条件为：
1. 下一个值是否为  -1；
2. 下一个值是否为 0。

（其实只用判断下一个值是否为 0 就行了，不为 0 就转向）
## 算法设计
生成螺旋矩阵就需要个转向操作，转向操作也十分的简单。

我们先定义一个转向用的结构体（内部类），包含行（x），列（y）转向信息。

<font color=#999AAA >c++代码如下（示例）：

```cpp
struct Position{
    int x;
    int y;
};
```


<font color=#999AAA >java代码如下（示例）：

```java
public class Position{
    int x;
    int y;
    Position(){}
    Position(int x,int y){
        this.x = x;
        this.y = y;
    }
}
```


### 转向操作

1. <font color=#CC3300>**向右：**</font>行 +0，列 +1；
2. <font color=#CC3300>**向下：**</font>行 +1，列 +0；
3. <font color=#CC3300>**向左：**</font>行 +0，列 -1；
4. <font color=#CC3300>**向上：**</font>行 -1，列 +0。



<font color=#999AAA >c++代码如下（示例）：

```cpp
Position DIR[4] = {0,1,1,0,0,-1,-1,0};
```


<font color=#999AAA >java代码如下（示例）：

```java
Position dir[] = {
    new Position(0,1),
   	new Position(1,0),
    new Position(0,-1),
    new Position(-1,0)
};
```

### 初始化
将边界设为 -1，而其他位置为 0.


<font color=#999AAA >c++代码如下（示例）：

```cpp
void Init(int n){
    for(int i = 1;i<=n;i++){
        for(int j = 1;j<=n;j++){
            m[i][j] = 0;
        }
    }
    for(int i = 0;i<=n+1;i++){
        m[0][i] = m[n+1][i] = -1;
    }
    for(int i = 0;i<=n+1;i++){
        m[i][0] = m[i][n+1] = -1;
    }
}
```


<font color=#999AAA >java代码如下（示例）：

```java
public void init(int n){
    for(int i = 1;i<=n;i++){
        for(int j = 1;j<=n;j++){
            m[i][j] = 0;
        }
    }
    for(int i = 0;i<=n+1;i++){
        m[0][i] = m[n+1][i] = -1;
    }
    for(int i = 0;i<=n+1;i++){
        m[i][0] = m[i][n+1] = -1;
    }
}
```


### 完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;

struct Position{
    int x;
    int y;
};
Position DIR[4] = {0,1,1,0,0,-1,-1,0};
Position here,nextt;

int m[100][100];
void Init(int n){
    for(int i = 1;i<=n;i++){
        for(int j = 1;j<=n;j++){
            m[i][j] = 0;
        }
    }
    for(int i = 0;i<=n+1;i++){
        m[0][i] = m[n+1][i] = -1;
    }
    for(int i = 0;i<=n+1;i++){
        m[i][0] = m[i][n+1] = -1;
    }
}

void Solve(int n){
    here.x = 1;
    here.y = 1;
    int num = 1;
    m[1][1] = 1;
    int dirIndex = 0;
    while(num < n*n){
        nextt.x = here.x + DIR[dirIndex].x;
        nextt.y = here.y + DIR[dirIndex].y;
        if(m[nextt.x][nextt.y] == 0){
            m[nextt.x][nextt.y] = ++num;
            here = nextt;
        }else{
            dirIndex = (dirIndex + 1) % 4;
        }
    }
}

void Print(int n){
    for(int i = 1;i<=n;i++){
        for(int j = 1;j<=n;j++){
            cout<<m[i][j]<<"\t";
        }
        cout<<endl;
    }
}

int main(){
    int n;cin>>n;
    Init(n);
    Solve(n);
    Print(n);
}
```


<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;
public class A {
    public class Position{
        int x;
        int y;
        Position(){}
        Position(int x,int y){
            this.x = x;
            this.y = y;
        }
    }
    public int m[][] = new int[100][100];
    public Position here = new Position();
    public Position nextt = new Position();

    Position dir[] = {
            new Position(0,1),
            new Position(1,0),
            new Position(0,-1),
            new Position(-1,0)
    };

    public void init(int n){
        for(int i = 1;i<=n;i++){
            for(int j = 1;j<=n;j++){
                m[i][j] = 0;
            }
        }
        for(int i = 0;i<=n+1;i++){
            m[0][i] = m[n+1][i] = -1;
        }
        for(int i = 0;i<=n+1;i++){
            m[i][0] = m[i][n+1] = -1;
        }
    }

    public void solve(int n){
        here.x = 1;
        here.y = 1;
        int num = 1;
        m[1][1] = 1;
        int dirIndex = 0;
        while(num < n*n){
            nextt.x = here.x + dir[dirIndex].x;
            nextt.y = here.y + dir[dirIndex].y;
            if(m[nextt.x][nextt.y] == 0){
                m[nextt.x][nextt.y] = ++num;
                here.x = nextt.x;
                here.y = nextt.y;
            }else{
                dirIndex = (dirIndex + 1) % 4;
            }
        }
    }

    public void print(int n){
        for(int i = 1;i<=n;i++){
            for(int j = 1;j<=n;j++){
                System.out.print(m[i][j] + "\t");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        A a = new A();
        int n;
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        a.init(n);
        a.solve(n);
        a.print(n);
    }
}
```


## 总结
今天的复习内容有丶简单。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



<font color=#999AAA >下期预告：</font>树-概述篇
