
`公众号`：URLeisure 的复习仓库

<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## 一、概述
* 深度优先搜索（Depth First Search，DFS）也是最常见的图搜索方法之一。
* 深度优先搜索沿着一条路径一直走下去，无法行进时，回退到刚刚访问的节点，像“不撞南墙不回头，不到黄河不死心”。
* 深度优先遍历是按照深度优先搜索的方式对图进行遍历。
* ` 深度优先遍历秘籍：`后被访问的顶点，其邻接点先被访问。
* 根据深度优先遍历秘籍，`后来先服务，可以借助于栈实现`。
* `递归本身就是用栈实现的`，因此使用递归方法更方便。
## 二、广度优先搜索
### 算法步骤
#### 递归
1. 初始化图中所有顶点未被访问；
2. 从图中某个顶点 v 出发，访问并标记 v 为已被访问；
3. `依次`检查 v 的所有邻接点 w，如果 w 未被访问，则从 w 出发进行深度优先遍历（递归调用，重复 2.3 步骤）

#### 非递归
1. 初始化图中所有顶点未被访问；
2. 从图中的某个顶点 v 出发，访问并标记 v 为已被访问；
3. 访问最近访问顶点的未被访问邻接点 w1，再访问 w1 的未被访问邻接点 w2……直到当前顶点没有未被访问的邻接点时停止；
4. 回退到最近访问过且有未被访问的邻接点的顶点，访问该顶点的未被访问的邻接点；
5. 重复 3.4 步骤，直到所有的顶点都被访问过。

* 深度优先遍历的递归算法和非递归算法虽然描述方式不同，但遍历的过程是一致的。
### 图解

![在这里插入图片描述](https://img-blog.csdnimg.cn/6a700ccdffc24d9b96c62cdf6e78d584.png)

在上图中，设 A 为源点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2d6ac35bb4d64cf3a29fc65b0b2859ca.gif)

* 从一个节点开始，遍历这个顶点的一个未被标记的邻接点，并标记该节点。
* 重复这个过程，直到当前节点没有未被标记的邻接点，返回上一邻接点，重复过程。

### BFS树
深度优先遍历经过的顶点及边，称为深度优先生成树，简称 `DFS 树`。

如图为图例的`深度优先生成树`。
![在这里插入图片描述](https://img-blog.csdnimg.cn/425c4feab58149a1b8222ec05542aaa3.png)

如果是非连通图，则每一个连通分量会产生一棵 `DFS树`，合在一起称为 `DFS森林`。

### 代码
#### 邻接矩阵实现
<font color=#999AAA >c++代码如下（示例）：

```cpp
void DFS_AM(AMGraph G, int v) {
    cout << G.Vex[v] << "\t";//输出 或 存储
    vis[v] = true;
    for (int w = 0; w < G.vexnum; w++) {
        if (G.Edge[v][w] && !vis[w]) {
            DFS_AM(G, w);
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void DFS_AM(AMGraph g, int v) {
    System.out.print(g.vex[v] + "\t");//输出 或 存储
    vis[v] = true;
    for (int w = 0; w < g.vexnum; w++) {
        if (g.edge[v][w] == 1 && !vis[w]) {
            DFS_AM(g, w);
        }
    }
}
```

#### 邻接表实现
<font color=#999AAA >c++代码如下（示例）：

```cpp
void DFS_Al(AlGraph G, int v) {
    cout << G.Vex[v].data << "\t";//输出 或 存储
    vis[v] = true;
    AdjNode *p = G.Vex[v].first;
    while (p) {
        int w = p->v;
        if (!vis[w]) {
            DFS_Al(G, w);
        }
        p = p->next;
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void DFS_Al(AlGraph g, int v) {
	System.out.print(g.vex[v].data + "\t");//输出 或 存储
    vis[v] = true;
    AdjNode p = g.vex[v].first;
    while (p != null) {
        int w = p.v;
        if (!vis[w]) {
            DFS_Al(g, w);
        }
        p = p.next;
    }
}
```
#### 链式前向星实现
<font color=#999AAA >c++代码如下（示例）：

```cpp
void DFS(int u) {
    cout << u << "\t";//输出 或 存储
    vis[u] = true;
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (!vis[v]) {
            DFS(v);
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static void DFS(int u) {
	System.out.print(u + "\t");//输出 或 存储
    vis[u] = true;
    for (int i = head[u]; i != -1; i = e[i].next) {
        int v = e[i].to;
        if (!vis[v]) {
            DFS(v);
        }
    }
}
```
## 三、完整代码

### 邻接矩阵版
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
bool vis[MaxVnum];

void Init() {
    memset(vis, false, sizeof vis);
}

int LocateVex(AMGraph G, VexType x) {
    for (int i = 0; i < G.vexnum; i++) {
        if (G.Vex[i] == x) {
            return i;
        }
    }
    return -1;
}

void CreateAM(AMGraph &G) {
    cin >> G.vexnum >> G.edgenum;
    VexType u, v;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i];
    }
    for (int i = 0; i < G.vexnum; i++) {
        for (int j = 0; j < G.vexnum; j++) {
            G.Edge[i][j] = 0;
        }
    }
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            G.Edge[i][j] = G.Edge[j][i] = 1;
        }
    }
}

void DFS_AM(AMGraph G, int v) {
    cout << G.Vex[v] << "\t";//输出 或 存储
    vis[v] = true;
    for (int w = 0; w < G.vexnum; w++) {
        if (G.Edge[v][w] && !vis[w]) {
            DFS_AM(G, w);
        }
    }
}

int main() {
    AMGraph G;
    Init();
    CreateAM(G);
    VexType v;
    cin >> v;
    int index = LocateVex(G, v);
    DFS_AM(G, index);
    return 0;
}
/*
8 10
A B C D E F G H
A B
A D
B C
B F
B G
C D
D E
D H
E H
F G
A
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class  A {
    public static final int MaxVnum = 100;
    public static Scanner sc = new Scanner(System.in);

    public static class AMGraph {
        String vex[] = new String[MaxVnum];
        int edge[][] = new int[MaxVnum][MaxVnum];
        int vexnum, edgenum;
    }

    public static boolean vis[] = new boolean[MaxVnum];

    public static void init() {
        Arrays.fill(vis, false);
    }

    public static int locateVex(AMGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].equals(x)) {
                return i;
            }
        }
        return -1;
    }

    public static void createAM(AMGraph g) {
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
            }
        }
    }

    public static void DFS_AM(AMGraph g, int v) {
        System.out.print(g.vex[v] + "\t");//输出 或 存储
        vis[v] = true;
        for (int w = 0; w < g.vexnum; w++) {
            if (g.edge[v][w] == 1 && !vis[w]) {
                DFS_AM(g, w);
            }
        }
    }

    public static void main(String[] args) {
        AMGraph g = new AMGraph();
        createAM(g);
        String v = sc.next();
        int index = locateVex(g, v);
        DFS_AM(g, index);
    }
}
/*
8 10
A B C D E F G H
A B
A D
B C
B F
B G
C D
D E
D H
E H
F G
A
 */
```

### 邻接表版
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include "cstring"

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
bool vis[MaxVnum];

void Inid() {
    memset(vis, false, sizeof vis);
}

int LocateVex(AlGraph G, VexType x) {
    for (int i = 0; i < G.vexnum; i++) {
        if (G.Vex[i].data == x) {
            return i;
        }
    }
    return -1;
}

void InsertEdge(AlGraph &G, int i, int j) {
    AdjNode *p = new AdjNode;
    p->v = j;
    p->next = G.Vex[i].first;
    G.Vex[i].first = p;
}

void CreateAl(AlGraph &G) {
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
            InsertEdge(G, j, i);
        }
    }
}

void DFS_Al(AlGraph G, int v) {
    cout << G.Vex[v].data << "\t";//输出 或 存储
    vis[v] = true;
    AdjNode *p = G.Vex[v].first;
    while (p) {
        int w = p->v;
        if (!vis[w]) {
            DFS_Al(G, w);
        }
        p = p->next;
    }
}

int main() {
    AlGraph G;
    Inid();
    CreateAl(G);
    VexType v;
    cin >> v;
    int index = LocateVex(G, v);
    DFS_Al(G, index);

    return 0;
}
/*
8 10
A B C D E F G H
A D
A B
B G
B F
B C
C D
D H
D E
E H
F G
A
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class A {
    public static final int MaxVnum = 100;
    public static Scanner sc = new Scanner(System.in);

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

    public static boolean vis[] = new boolean[MaxVnum];

    public static void init() {
        Arrays.fill(vis, false);
    }

    public static int locateVex(AlGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].data.equals(x)) {
                return i;
            }
        }
        return -1;
    }

    public static void insert(AlGraph g, int i, int j) {
        AdjNode p = new AdjNode();
        p.v = j;
        p.next = g.vex[i].first;
        g.vex[i].first = p;
    }

    public static void createAl(AlGraph g) {
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
                insert(g, i, j);
                insert(g, j, i);
            }
        }
    }

    public static void DFS_Al(AlGraph g, int v) {
        System.out.print(g.vex[v].data + "\t");//输出 或 存储
        vis[v] = true;
        AdjNode p = g.vex[v].first;
        while (p != null) {
            int w = p.v;
            if (!vis[w]) {
                DFS_Al(g, w);
            }
            p = p.next;
        }
    }

    public static void main(String[] args) {
        AlGraph g = new AlGraph();
        init();
        createAl(g);
        String v = sc.next();
        int index = locateVex(g, v);
        DFS_Al(g, index);
    }
}
/*
8 10
A B C D E F G H
A D
A B
B G
B F
B C
C D
D H
D E
E H
F G
A
 */
```
### 链式前向星版
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include "iostream"
#include "cstring"

using namespace std;
const int N = 1e3;

struct Edge {
    int to;
    int next;
} e[N];
int head[N], cnt;
int n, m;
bool vis[N];

void Init() {
    memset(head, -1, sizeof head);
    memset(vis, false, sizeof vis);
    cnt = 0;
}

void add(int u, int v) {
    e[cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void DFS(int u) {
    cout << u << "\t";//输出 或 存储
    vis[u] = true;
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (!vis[v]) {
            DFS(v);
        }
    }
}

int main() {
    cin >> n >> m;
    Init();
    int u, v;
    for (int i = 0; i < m; i++) {
        cin >> u >> v;
        add(u, v);
        add(v, u);
    }
    cin >> u;
    DFS(u);
    return 0;
}
/*
8 10
1 4
1 2
2 7
2 6
2 3
3 4
4 8
4 5
5 8
6 7
1
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class A {
    public static final int N = 100;
    public static Scanner sc = new Scanner(System.in);

    public static class Edge {
        int to;
        int next;
    }

    public static Edge e[] = new Edge[N];
    public static boolean vis[] = new boolean[N];
    public static int head[] = new int[N];
    public static int cnt;

    public static void init() {
        Arrays.fill(vis, false);
        Arrays.fill(head, -1);
        cnt = 0;
    }

    public static void add(int u, int v) {
        e[cnt] = new Edge();
        e[cnt].to = v;
        e[cnt].next = head[u];
        head[u] = cnt++;
    }

    public static void DFS(int u) {
        System.out.print(u + "\t");//输出 或 存储
        vis[u] = true;
        for (int i = head[u]; i != -1; i = e[i].next) {
            int v = e[i].to;
            if (!vis[v]) {
                DFS(v);
            }
        }
    }

    public static void main(String[] args) {
        init();
        int n = sc.nextInt();
        int m = sc.nextInt();
        int u, v;
        for (int i = 0; i < m; i++) {
            u = sc.nextInt();
            v = sc.nextInt();
            add(u, v);
            add(v, u);
        }
        v = sc.nextInt();
        DFS(v);
    }
}
/*
8 10
1 4
1 2
2 7
2 6
2 3
3 4
4 8
4 5
5 8
6 7
1
 */
```
## 四、总结

### 算法复杂度分析
>深度优先搜索的过程实质上是对每个顶点搜索其邻接点的过程，图的存储方式不同，其算法复杂度也不同。

#### 基于邻接矩阵的 DFS 算法
**时间复杂度：**
* 查找每个顶点的邻接点需要 $O(n)$ 时间，一共 n 个顶点，总`时间复杂度为`$O(n^2)$ 。

**空间复杂度：**
* 使用了一个递归工作栈，`空间复杂度为` $O(n)$ 。

#### 基于邻接表的 DFS 算法
**时间复杂度：**
* 查找顶点 V~i~ 的邻接点需要 $O(d(V_i))$ 时间，$d(V_i)$ 为 V~i~ 的出度（无向图为度）。
* 对有向图，所有顶点的出度之和等于边数 e；对无向图，所有顶点的度之和为 2e。
* 因此查找邻接点的时间复杂度为 $O(n)$，加上初始化时间 $O(n)$，总的`时间复杂度为` $O(n+e)$。

**空间复杂度：**
* 使用了一个递归工作栈，`空间复杂度为`$O(n)$。

### 注意
>图的邻接矩阵是唯一的，因此`基于邻接矩阵的 BFS 或 DFS 遍历序列也是唯一的`。

>图的邻接表不是唯一的，边的输入顺序不同，正序或逆序建表都会影响邻接表的邻接点顺序，因此`基于邻接表的 BFS 或 DFS 遍历序列不是唯一的`。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>图的连通性
