<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



## 一、概述
图的结构比较复杂，任何两个顶点之间都可能有关系。

* `顺序存储`，则需要使用二维数组表示元素之间的关系，即`邻接矩阵（adjacency matrix），也可以使用边集数组`，把每条边顺序存储起来。

* `链式存储`，有`邻接表、十字链表、多重表和链式前向星`等表示方法。

其中，邻接矩阵和邻接表是最简单、最常用的存储方法。（竞赛用链式前向星多）
## 二、邻接矩阵
### 1. 存储方法
>一个一维数组存储图中顶点的信息。

>一个二维数组存储图中顶点之间的邻接关系，存储顶点之间邻接关系的二维数组称为`邻接矩阵`。

### 2. 表示方法
#### 1). 无向图
在无向图中，如果 V~i~ 和 V~j~，则邻接矩阵 $M[i][j] = M[j][i] = 1$,否则 $M[i][j] = M[j][i] = 0$。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc8404c373e246b6abcd4d66e842f295.png)


如图，存储此无向图，其顶点信息和邻接矩阵如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/66f1d2a975b147b78d9c4de57fe9350c.png)

##### 特点
1. 无向图的邻接矩阵是对称矩阵，并且是唯一的；
2. 第 i 行或第 i 列非零元素个数正好是第 i 个顶点的度。
#### 2). 有向图
在有向图中，如果 V~i~ 到 V~j~ 有弧，则邻接矩阵 $M[i][j] = 1$，否则 $M[i][j] = 0$。

![在这里插入图片描述](https://img-blog.csdnimg.cn/22ee89c489b54887b5b37f7e1ea619f4.png)
                                                                         
`注意：`<V~i~,V~j~> 表示有序对，(V~i~,V~j~) 表示无序对。

如图，存储此有向图，其顶点信息和邻接矩阵如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fbb2c13d47f640ee9c4581b85f3762fd.png)


##### 特点
1. 有向图的邻接矩阵不一定是对称的；
2. 第 i 行非零元素的个数正好是第 i 个顶点的出度，第 i 列非零元素的个数正好是第 i 个顶点的入度。

#### 3). 网
网是带权的图，需要存储边的权值，邻接矩阵表示为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa18462d92f042a1a6e15edf8b29884d.png)
                                                                         
其中，W~ij~ 表示边的权值，∞ 表示无穷大。

当 $i = j$时，W~ii~ 也可以设置为 0。

如图，存储此网，其顶点信息和邻接矩阵如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/e57f1c63a78b4c91b6eb7153a3106563.png)


## 三、实现
做个无向图的邻接矩阵，其他的就不做了，都一样。
### 1. 邻接矩阵的数据结构
<font color=#999AAA >c++代码如下（示例）：

```cpp
#define MaxVnum 100//顶点数最大值
typedef char VexType;//顶点类型
typedef int EdgeType;//边类型
struct AMGraph {
    VexType Vex[MaxVnum];
    EdgeType Edge[MaxVnum][MaxVnum];
    int vexnum, edgenum;//顶点数，边数
};
```

<font color=#999AAA >java代码如下（示例）：

```java
private static class AMGraph {
    String vex[] = new String[maxVnum];
    int edge[][] = new int[maxVnum][maxVnum];
    int vexnum, edgenum;
}
```
### 2. 算法步骤
<font color=#CC3300>**算法步骤：**</font>
1. 输入顶点数和边数；
2. 依次输入顶点信息，存储到顶点数组 Vex[ ] 中；
3. 初始化邻接矩阵，如果是图，则初始化为 0；如果是网，则初始化为 ∞；
4. 依次输入每条边依附的两个顶点，如果是网，还要输入该边的权值。

* `无向图：`$G.Edge[i][j] = G.Edge[j][i] = 1;$
* `有向图：`$G.Edge[i][j] = 1;$
* `无向网：`$G.Edge[i][j] = G.Edge[j][i] = w;$
* `有向网：`$G.Edge[i][j] = w;$
* 
### 3.代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateGraph(AMGraph &G) {
    cout << "输入顶点数和边数：" << endl;
    cin >> G.vexnum >> G.edgenum;
    cout << "输入顶点信息" << endl;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i];
    }
    memset(G.Edge, 0, sizeof G.Edge);//初始化为 0（网初始化为 ∞）
    cout << "输入每条边依附的两个顶点：" << endl;
    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);//查找位置
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {//存在
            G.Edge[i][j] = G.Edge[j][i] = 1;
        } else {
            cout << "错误!" << endl;
            G.edgenum++;
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void createGraph(AMGraph g) {
    g.vexnum = sc.nextInt();
    g.edgenum = sc.nextInt();
    for (int i = 0; i < g.vexnum; i++) {
        g.vex[i] = sc.next();
   	}
    for (int i = 0; i < g.vexnum; i++) {
       	for (int j = 0; j < g.vexnum; j++) {
            g.edge[i][j] = 0;
       }
    }
    String u, v;
  	while (g.edgenum-- > 0) {
        u = sc.next();
        v = sc.next();
        int i = locateVex(g, u);
        int j = locateVex(g, v);
        if (i != -1 && j != -1) {
            g.edge[i][j] = g.edge[j][i] = 1;
        } else {
            System.out.println("错误！");
            g.edgenum++;
        }
    }
}
```
## 四、完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include <cstring>

using namespace std;
#define MaxVnum 100
typedef char VexType;
typedef int EdgeType;
struct AMGraph {
    VexType Vex[MaxVnum];
    EdgeType Edge[MaxVnum][MaxVnum];
    int vexnum, edgenum;
};

int LocateVex(AMGraph G, int x) {
    for (int i = 0; i < G.vexnum; i++) {
        if (G.Vex[i] == x) {
            return i;
        }
    }
    return -1;
}

void CreateGraph(AMGraph &G) {
    cout << "输入顶点数和边数：" << endl;
    cin >> G.vexnum >> G.edgenum;
    cout << "输入顶点信息" << endl;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i];
    }
    memset(G.Edge, 0, sizeof G.Edge);
    cout << "输入每条边依附的两个顶点：" << endl;
    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            G.Edge[i][j] = G.Edge[j][i] = 1;
        } else {
            cout << "错误!" << endl;
            G.edgenum++;
        }
    }
}

void Print(AMGraph G) {
    for (int i = 0; i < G.vexnum; i++) {
        for (int j = 0; j < G.vexnum; j++) {
            cout << G.Edge[i][j] << " ";
        }
        cout << endl;
    }
}

int main() {
    AMGraph G;
    CreateGraph(G);
    Print(G);
}
/*
4 5
a b c d
a b
a c
a d
b d
c d
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);
    private static final int maxVnum = 100;

    private static class AMGraph {
        String vex[] = new String[maxVnum];
        int edge[][] = new int[maxVnum][maxVnum];
        int vexnum, edgenum;
    }

    public static int locateVex(AMGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].equals(x)) {
                return i;
            }
        }
        return -1;
    }

    public static void createGraph(AMGraph g) {
        g.vexnum = sc.nextInt();
        g.edgenum = sc.nextInt();
        for (int i = 0; i < g.vexnum; i++) {
            g.vex[i] = sc.next();
        }
        for (int i = 0; i < g.vexnum; i++) {
            for (int j = 0; j < g.vexnum; j++) {
                g.edge[i][j] = 0;
            }
        }
        String u, v;
        while (g.edgenum-- > 0) {
            u = sc.next();
            v = sc.next();
            int i = locateVex(g, u);
            int j = locateVex(g, v);
            if (i != -1 && j != -1) {
                g.edge[i][j] = g.edge[j][i] = 1;
            } else {
                System.out.println("错误！");
                g.edgenum++;
            }
        }
    }

    public static void print(AMGraph g) {
        for (int i = 0; i < g.vexnum; i++) {
            for (int j = 0; j < g.vexnum; j++) {
                System.out.print(g.edge[i][j] + " ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        AMGraph g = new AMGraph();
        createGraph(g);
        print(g);
    }

}
/*
4 5
a b c d
a b
a c
a d
b d
c d
 */
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


<font color=#999AAA >下期预告：</font>图的存储_邻接表
