<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 一、概述
* 邻接表（Adjacency LIst）是图的一种链式存储方法。
* 邻接表包含两部分：`顶点`和`临界点`。
* `顶点`包括顶点信息和指向第一个邻接点的指针。
* `邻接点`包括邻接点的存储下标和指向下一个邻接点的指针。顶点 v~i~ 的所有邻接点构成一个单链表。

## 二、邻接表的表示方法

### 1. 无向图邻接表
以下图无向图为例，创建邻接表如图。

![无向图邻接表](https://img-blog.csdnimg.cn/007ed5e971a34663b49144e8c6d5be5c.png)

#### 1). 解释
* a 的邻接点是 b、d，其邻接点的存储下表为 1、3，按照头插法（逆序）将其放入 a 后面的单链表中；
* b 的邻接点是 a、c、d，其邻界点的存储下标为 0、2、3，将其放入 b 后边的单链表中；
* **……**

#### 2). 特点
1. 无向图有 n 个顶点和 e 条边，则顶点表有 n 个节点，邻接点表有 2e 个节点；
2. `顶点的度`为该顶点后面单链表中的节点数。

### 2. 有向图的邻接表
以下图有向图为例，创建邻接表如图。
                                                                         
![有向图邻接表](https://img-blog.csdnimg.cn/66ead0f19806463db6063e255feeec9f.png)

#### 1). 解释
`只看出度：`
* a 的邻接点（只看出度）是 b、d，其邻接点的储存下标为 1、3，按头插法（逆序）将其放入 a 后面的单链表中；
* b 的邻接点是 c，其邻接点的存储下标为 2，将其放入 b 后面的单链表中；
* c 没有邻接点，其后面单链表为空；
* **……**

#### 2). 特点
1. 有向图有 n 个顶点，e 条边，则顶点表有 n 个节点，邻接点表有 e 个节点；
2. `顶点的出度`为该顶点后面单链表中的节点数。

### 3. 有向图的逆邻接表
以下图有向图为例，创建逆邻接表如图。

![逆邻接表](https://img-blog.csdnimg.cn/b4ec1c9e2b8947fdaddebe3667ee62f4.png)

#### 1). 解释
`逆邻接表是按照入度建表。`
* a 没有逆邻接点（只看入度），其后面得单邻接表为空；
* b 得逆邻接点是 a、d，其存储下标为 0、3，按头插法将其放入 b 后面的单链表中；
* **……** 
#### 2). 特点
1. 有向图有 n 个顶点，e 条边，则顶点表有 n 个节点，邻接点表有 e 个节点；
2. `顶点的入度`为该顶点后面单链表中的节点数。
## 三、有向图的创建
### 1. 数据结构的定义
邻接表用到 2 个数据结构。

1. `顶点节点：`包括顶点信息和指向第一个邻接点得指针，可用一维数组存储；
2. `邻接点节点：`包括邻接点的存储下标和指向下一个邻接点得指针。顶点 v~i~ 的所有邻接点构成一个单链表。（如果是网，则再加一个存放权的储存空间）

![示例](https://img-blog.csdnimg.cn/cf152f66bfb34636a3f3484765e10be4.png)
                                                                         
<font color=#999AAA >c++代码如下（示例）：

```cpp
struct AdjNode {//邻接点结构体
    int v;
    AdjNode *next;
};

struct VexNode {//节点结构体
    VexType data;
    AdjNode *first;//first 指向邻接点
};

struct AlGraph {//节点数组结构体
    VexNode Vex[MaxVnum];
    int vexnum, edgenum;
};
```

<font color=#999AAA >java代码如下（示例）：

```java
public static class AdjNode {
    int v;
    AdjNode next;
}

public static class VexNode {
    String data;
    AdjNode first;
}

public static class AlGraph {
    VexNode vex[] = new VexNode[MaxVnum];
    int vexnum, edgenum;
}
```

### 2. 邻接表存储方法
<font color=#CC3300>**算法步骤：**</font>
1. 输入顶点数和边数；
2. 依次输入顶点信息，存储到顶点数组 Vex[ ] 的 data 域中，Vex[ ] 的 first 域置空；
3. 依次输入每条边依附的两个顶点，如果是网，还需要输入该边的权。
4. 查询两个顶点在 Vex[ ] 中的储存下标 i、j
	1. `无向图：`创建新邻接点 s1，令 s1 -> v = j; s1 -> next = NULL;然后将 s1 节点插入第 i 个顶点的第一个邻接表之前（头插法）。 再创建新邻接点 s2，令 s2 -> v = i; s2 -> next = NULL;然后将 s2 节点插入第 j 个顶点的第一个邻接表之前（头插法）；
	2. `有向图：`创建新邻接点 s，令 s -> v = j; s -> next = NULL;然后将 s1 节点插入第 i 个顶点的第一个邻接表之前（头插法）。 

### 3. 代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateGraph(AlGraph &G) {
    cin >> G.vexnum >> G.edgenum;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i].data;
        G.Vex[i].first = NULL;//指向空
    }

    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            InsertEdge(G, i, j);//头插法
        } else {//不存在该节点
            cout << "错误" << endl;
            G.edgenum++;
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void createGraph(AlGraph g) {
    g.vexnum = sc.nextInt();
    g.edgenum = sc.nextInt();
    for (int i = 0; i < g.vexnum; i++) {
        g.vex[i] = new VexNode();
        g.vex[i].data = sc.next();
        g.vex[i].first = null;
    }
    String u, v;
  	while (g.edgenum-- > 0) {
        u = sc.next();
        v = sc.next();
        int i = locateVex(g, u);
        int j = locateVex(g, v);
        if (i != -1 && j != -1) {
            insertEdge(g, i, j);
        } else {
            System.out.println("错误");
            g.edgenum++;
       	}
    }
}
```
## 四、完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>

using namespace std;
#define MaxVnum 100
typedef char VexType;

struct AdjNode {
    int v;
    AdjNode *next;
};

struct VexNode {
    VexType data;
    AdjNode *first;
};

struct AlGraph {
    VexNode Vex[MaxVnum];
    int vexnum, edgenum;
};

int LocateVex(AlGraph G, VexType x) {
    for (int i = 0; i < G.vexnum; i++) {
        if (G.Vex[i].data == x) {
            return i;
        }
    }
    return -1;
}

void InsertEdge(AlGraph &G, int i, int j) {
    AdjNode *s = new AdjNode;
    s->v = j;
    s->next = G.Vex[i].first;
    G.Vex[i].first = s;
}

void CreateGraph(AlGraph &G) {
    cin >> G.vexnum >> G.edgenum;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i].data;
        G.Vex[i].first = NULL;
    }

    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            InsertEdge(G, i, j);
        } else {
            cout << "错误" << endl;
            G.edgenum++;
        }
    }
}

void Print(AlGraph G) {
    cout << "如下" << endl;
    for (int i = 0; i < G.vexnum; i++) {
        cout << G.Vex[i].data;
        AdjNode *t = G.Vex[i].first;
        while (t != NULL) {
            cout << "->[" << t->v << "]";
            t = t->next;
        }
        cout << endl;
    }
}

int main() {
    AlGraph G;
    CreateGraph(G);
    Print(G);
    return 0;
}
/*
4 5
a b c d
a b
a d
b c
d b
d c
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    private static final int MaxVnum = 100;
    private static Scanner sc = new Scanner(System.in);

    public static class AdjNode {
        int v;
        AdjNode next;
    }

    public static class VexNode {
        String data;
        AdjNode first;
    }

    public static class AlGraph {
        VexNode vex[] = new VexNode[MaxVnum];
        int vexnum, edgenum;
    }

    public static int locateVex(AlGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].data.equals(x)) {
                return i;
            }
        }
        return -1;
    }

    public static void insertEdge(AlGraph g, int i, int j) {
        AdjNode s = new AdjNode();
        s.v = j;
        s.next = g.vex[i].first;
        g.vex[i].first = s;
    }

    public static void createGraph(AlGraph g) {
        g.vexnum = sc.nextInt();
        g.edgenum = sc.nextInt();
        for (int i = 0; i < g.vexnum; i++) {
            g.vex[i] = new VexNode();
            g.vex[i].data = sc.next();
            g.vex[i].first = null;
        }
        String u, v;
        while (g.edgenum-- > 0) {
            u = sc.next();
            v = sc.next();
            int i = locateVex(g, u);
            int j = locateVex(g, v);
            if (i != -1 && j != -1) {
                insertEdge(g, i, j);
            } else {
                System.out.println("错误");
                g.edgenum++;
            }
        }
    }

    public static void print(AlGraph g) {
        for (int i = 0; i < g.vexnum; i++) {
            System.out.print(g.vex[i].data);
            AdjNode t = g.vex[i].first;
            while (t != null) {
                System.out.print("->[" + t.v + "]");
                t = t.next;
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        AlGraph g = new AlGraph();
        createGraph(g);
        print(g);
    }
}
/*
4 5
a b c d
a b
a d
b c
d b
d c
 */
```
## 五、总结
### 1. 优点
* 便于增删顶点；
* 便于访问所有邻接点；
* 访问所有顶点的邻接点，`时间复杂度为 O(n+e)`；
* 空间复杂度低。顶点占用 n 个空间，无向图的邻接点表占用 n+2e 个空间，有向图的邻接点表占用 n+e 个空间，总体`空间复杂度为 O(n+e)`，而邻接矩阵的空间复杂度为 $O(n^2)$；
* `因此对于稀疏图可以采用邻接表存储，对于稠密图可以采用邻接矩阵存储`。
### 2. 缺点
* 不便于判断两顶点之间是否有边。要判断两顶点是否有边，需要遍历该顶点后面的邻接点链表；
* 不便于计算各顶点的度；

### 3. 综合
* 虽然邻接表访问单个邻接点的效率不高，但是访问一个顶点的所有邻接点，仅需访问该顶点后面的单链表即可，`时间复杂度为该顶点的度 O(d(v~i~))`；
* 而邻接矩阵访问一个顶点的所有邻接点，`时间复杂度为 O(n)`；
* `总体上邻接表比邻接矩阵效率更高`；
* 有向图邻接表求出度容易，而逆邻接表求出度容易，如果想快速求顶点的出度和入度，`可以将邻接表和逆邻接表结合起来，采用十字链表存储`。


<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


<font color=#999AAA >下期预告：</font>图的存储_边集数组
