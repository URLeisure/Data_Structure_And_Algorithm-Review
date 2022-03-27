<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## 一、概述
* 图的遍历和树的遍历类似，是从图的某一顶点出发，按照某种搜索方式对图中所有的顶点访问一次且仅一次。
* 图的遍历可以解决很多搜索问题，在实际中应用非常广泛。
* 图的遍历根据搜索方式的不同，分为：`广度优先搜索`和`深度优先搜索`。
## 二、广度优先搜索

* `广度优先搜索（Breadth First Search，BFS）`，又称宽度优先搜索，是最常见的图搜索方法之一。
* 广度优先搜索是从某个顶点（源点）出发，一次性访问所有未被访问的邻接点，再依次从这些访问过的邻接点出发。
* 广度优先遍历是按照广度优先搜索的方式对图进行遍历。
### 图解
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f2b480f91f249dd83fb7fa722f5d26f.png)

在上图中，设 A 为源点。
1. 从 A 出发，访问 A 的邻接点 B、C；
2. 再从 B 出发，访问 B 的邻接点 D、E；
3. 从 C 出发，C 的邻接点 B、E 已被访问；
4. 从 D 出发，D 没有邻接点；
5. 从 E 出发，E 的邻接点 D 已被访问；
6. 访问完毕。

访问路径如图。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/4cb5d9d1605045379bb2fff02e67f16f.png)
                                                                         
规律：`先被访问的顶点，其邻接点先被访问`。

根据这个规律，我们可以用队列来实现广度优先遍历。

创建 vis[ ]，来标记已被访问的顶点。

### BFS树
广度优先遍历经过的顶点及边，称为`广度优先生成树`，简称 `BFS树`。

如图为图例的`广度优先生成树`。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/8920fbe7fc8542ffa59fcfb78dd9193b.png)

如果是非连通图，则每一个连通分量会产生一棵 `BFS树`，合在一起称为 `BFS森林`。


### 代码

`算法步骤：`
1. 初始化图的所有顶点未被访问，初始化一个空队列；
2. 从图的某个顶点 V 出发，访问 V 并标记已访问，将 V 入队；
3. 如果队列非空，则继续执行，否则算法结束；
4. 队头元素 V 出队，依次访问 V 的所有未被访问的邻接点，标记已访问并入队，转向步骤3。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1f2b480f91f249dd83fb7fa722f5d26f.png)

上图中示例：
1. A 入队；
2. A 出队，判断 A 的邻接点；
3. A 的邻接点 B、C 入队；
4. B 出队，判断 B 的邻接点；
5. B 的邻接点 D、E 入队；
6. C 出队，判断 C 的邻接点，都被标记不如队列；
7. D 出队，判断 D 的邻接点；
8. E 出队，判断 E 的邻接点。
9. 队列为空，终止。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8956d371a9cf4017bd9b9062cceee9d8.gif)
                                                                         
（这是邻接矩阵的结果，如果是邻接表的话，结果是ACBED。因为邻接表的添加节点类似于头插法，后输入的在前面。）

初始化见完整代码。

#### 邻接矩阵实现

<font color=#999AAA >c++代码如下（示例）：

```cpp
void BFS_AM(AMGraph G, VexType v) {
    queue<VexType> Q;
    int i = LocateVex(G, v);
    vis[i] = true;
    Q.push(v);
    while (!Q.empty()) {
        VexType u = Q.front();
        cout << u << "\t";//打印，也可以存到数组里，再打印数组
        i = LocateVex(G, u);
        Q.pop();
        for (int j = 0; j < G.vexnum; j++) {
            if (G.Edge[i][j] && !vis[j]) {
                vis[j] = true;
                Q.push(G.Vex[j]);
            }
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
private static void BFS_AM(AMGraph g, String v) {
	LinkedList<String> queue = new LinkedList<>();
    int i = locateVex(g, v);
    vis[i] = true;
    queue.add(v);
    while (!queue.isEmpty()) {
        String u = queue.pop();
        System.out.print(u + "\t");
        i = locateVex(g, u);
        for (int j = 0; j < g.vexnum; j++) {
            if (g.edge[i][j] != 0 && !vis[j]) {
                vis[j] = true;
                queue.add(g.vex[j]);
            }
        }
    }
}
```
#### 邻接表实现

<font color=#999AAA >c++代码如下（示例）：

```cpp
void BFS_AL(ALGraph G, VexType v) {
    queue<VexType> Q;
    int i = LocateVex(G, v);
    vis[i] = true;
    Q.push(v);
    while (!Q.empty()) {
        VexType u = Q.front();
        cout << u << "\t";
        Q.pop();
        i = LocateVex(G, u);
        AdjNode *p = G.Vex[i].first;
        while (p) {
            if (!vis[p->v]) {
                Q.push(G.Vex[p->v].data);
                vis[p->v] = true;
            }
            p = p->next;
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
private static void BFS_AL(ALGraph g, String v) {
    LinkedList<String> queue = new LinkedList<>();
    int i = locateVex(g, v);
    vis[i] = true;
    queue.add(v);
 	while (!queue.isEmpty()) {
       	String u = queue.pop();
        System.out.print(u + "\t");
        i = locateVex(g, u);
        AdjNode p = g.vex[i].first;
       	while (p != null) {
            if (!vis[p.v]) {
                vis[p.v] = true;
                queue.add(g.vex[p.v].data);
            }
            p = p.next;
        }
    }
}
```

## 三、完整代码
### 邻接矩阵版
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include "iostream"
#include "cstring"
#include "queue"

using namespace std;

typedef char VexType;
typedef int EdgeType;
#define MAXN 100//注意，开太大的话二维数组受不了

struct AMGraph {
    VexType Vex[MAXN];
    EdgeType Edge[MAXN][MAXN];
    int vexnum, edgenum;
};

bool vis[MAXN];

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

void CreateAMGraph(AMGraph &G) {
    cin >> G.vexnum >> G.edgenum;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i];
    }
    for (int i = 0; i < G.vexnum; i++) {
        for (int j = 0; j < G.vexnum; j++) {
            G.Edge[i][j] = 0;
        }
    }
    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            G.Edge[i][j] = 1;
        } else {
            cout << "输入了不存在的节点，请重新输入" << endl;
            G.edgenum++;
        }
    }
}

void BFS_AM(AMGraph G, VexType v) {
    queue<VexType> Q;
    int i = LocateVex(G, v);
    vis[i] = true;
    Q.push(v);
    while (!Q.empty()) {
        VexType u = Q.front();
        cout << u << "\t";
        i = LocateVex(G, u);
        Q.pop();
        for (int j = 0; j < G.vexnum; j++) {
            if (G.Edge[i][j] && !vis[j]) {
                vis[j] = true;
                Q.push(G.Vex[j]);
            }
        }
    }
}

int main() {
    Init();
    AMGraph G;
    CreateAMGraph(G);
    VexType v;
    cin >> v;
    BFS_AM(G, v);
    return 0;
}
/*
5 7
A B C D E
A B
A C
B D
B E
C B
C E
E D
A
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);
    private static final int MAX = 100;

    private static class AMGraph {
        String vex[] = new String[MAX];
        int edge[][] = new int[MAX][MAX];
        int edgenum, vexnum;
    }

    private static boolean vis[] = new boolean[MAX];

    private static void init() {
        Arrays.fill(vis, false);
    }

    private static int locateVex(AMGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].equals(x)) {
                return i;
            }
        }
        return -1;
    }

    private static void createAMGraph(AMGraph g) {
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
                g.edge[i][j] = 1;
            } else {
                System.out.println("输入了不存在的节点，请重新输入");
                g.edgenum++;
            }
        }
    }

    private static void BFS_AM(AMGraph g, String v) {
        LinkedList<String> queue = new LinkedList<>();
        int i = locateVex(g, v);
        vis[i] = true;
        queue.add(v);
        while (!queue.isEmpty()) {
            String u = queue.pop();
            System.out.print(u + "\t");
            i = locateVex(g, u);
            for (int j = 0; j < g.vexnum; j++) {
                if (g.edge[i][j] != 0 && !vis[j]) {
                    vis[j] = true;
                    queue.add(g.vex[j]);
                }
            }
        }
    }

    public static void main(String[] args) {
        AMGraph g = new AMGraph();
        init();
        createAMGraph(g);
        String v = sc.next();
        BFS_AM(g, v);
    }
}
/*
5 7
A B C D E
A B
A C
B D
B E
C B
C E
E D
A
 */
```

### 邻接表版
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include "iostream"
#include "cstring"
#include "queue"

using namespace std;
typedef char VexType;
#define MAX 100

struct AdjNode {
    int v;
    AdjNode *next;
};

struct VexNode {
    VexType data;
    AdjNode *first;
};

struct ALGraph {
    VexNode Vex[MAX];
    int vexnum, edgenum;
};
bool vis[MAX];

void Init() {
    memset(vis, false, sizeof vis);
}

void InsertEdge(ALGraph &G, int i, int j) {
    AdjNode *s;
    s = new AdjNode;
    s->v = j;
    s->next = G.Vex[i].first;
    G.Vex[i].first = s;
}

int LocateVex(ALGraph G, VexType x) {
    for (int i = 0; i < G.vexnum; i++) {
        if (G.Vex[i].data == x) {
            return i;
        }
    }
    return -1;
}

void CreateALGraph(ALGraph &G) {
    cin >> G.vexnum >> G.edgenum;
    for (int i = 0; i < G.vexnum; i++) {
        cin >> G.Vex[i].data;
        G.Vex[i].first = nullptr;
    }
    VexType u, v;
    while (G.edgenum--) {
        cin >> u >> v;
        int i = LocateVex(G, u);
        int j = LocateVex(G, v);
        if (i != -1 && j != -1) {
            InsertEdge(G, i, j);
        } else {
            cout << "输入了不存在的节点，请重新输入" << endl;
            G.edgenum++;
        }
    }
}

void BFS_AL(ALGraph G, VexType v) {
    queue<VexType> Q;
    int i = LocateVex(G, v);
    vis[i] = true;
    Q.push(v);
    while (!Q.empty()) {
        VexType u = Q.front();
        cout << u << "\t";
        Q.pop();
        i = LocateVex(G, u);
        AdjNode *p = G.Vex[i].first;
        while (p) {
            if (!vis[p->v]) {
                Q.push(G.Vex[p->v].data);
                vis[p->v] = true;
            }
            p = p->next;
        }
    }
}

int main() {
    ALGraph G;
    Init();
    CreateALGraph(G);
    VexType v;
    cin >> v;
    BFS_AL(G, v);
    return 0;
}
/*
5 7
A B C D E
A B
A C
B D
B E
C B
C E
E D
A
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);
    private static final int MAX = 100;

    private static class AdjNode {
        int v;
        AdjNode next;
    }

    private static class VexNode {
        String data;
        AdjNode first;
    }

    private static class ALGraph {
        VexNode vex[] = new VexNode[MAX];
        int vexnum, edgenum;
    }

    private static boolean vis[] = new boolean[MAX];

    private static void init() {
        Arrays.fill(vis, false);
    }

    private static void insertEdge(ALGraph g, int i, int j) {
        AdjNode p = new AdjNode();
        p.v = j;
        p.next = g.vex[i].first;
        g.vex[i].first = p;
    }

    private static int locateVex(ALGraph g, String x) {
        for (int i = 0; i < g.vexnum; i++) {
            if (g.vex[i].data.equals(x)) {
                return i;
            }
        }
        return -1;
    }

    private static void createALGraph(ALGraph g) {
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
                System.out.println("输入了不存在的节点，请重新输入");
                g.edgenum++;
            }
        }
    }

    private static void BFS_AL(ALGraph g, String v) {
        LinkedList<String> queue = new LinkedList<>();
        int i = locateVex(g, v);
        vis[i] = true;
        queue.add(v);
        while (!queue.isEmpty()) {
            String u = queue.pop();
            System.out.print(u + "\t");
            i = locateVex(g, u);
            AdjNode p = g.vex[i].first;
            while (p != null) {
                if (!vis[p.v]) {
                    vis[p.v] = true;
                    queue.add(g.vex[p.v].data);
                }
                p = p.next;
            }
        }
    }

    public static void main(String[] args) {
        ALGraph g = new ALGraph();
        init();
        createALGraph(g);
        String v = sc.next();
        BFS_AL(g, v);
    }
}
/*
5 7
A B C D E
A B
A C
B D
B E
C B
C E
E D
A
 */
```

## 四、总结

### 算法复杂度分析
>广度优先搜索的过程实质上是对每个顶点搜索其邻接点的过程，图的存储方式不同，其算法复杂度也不同。

#### 基于邻接矩阵的 BFS 算法
**时间复杂度：**
* 查找每个顶点的邻接点需要 $O(n)$ 时间，一共 n 个顶点，总`时间复杂度为`$O(n^2)$ 。

**空间复杂度：**
* 使用了一个辅助队列，最坏的情况下每个顶点入队一次，`空间复杂度为` $O(n)$ 。

#### 基于邻接表的 BFS 算法
**时间复杂度：**
* 查找顶点 V~i~ 的邻接点需要 $O(d(V_i))$ 时间，$d(V_i)$ 为 V~i~ 的出度（无向图为度）。
* 对有向图，所有顶点的出度之和等于边数 e；对无向图，所有顶点的度之和为 2e。
* 因此查找邻接点的时间复杂度为 $O(n)$，加上初始化时间 $O(n)$，总的`时间复杂度为` $O(n+e)$。

**空间复杂度：**
* 使用了一个辅助队列，最坏的情况下每个顶点入队一次，`空间复杂度为`$O(n)$。

### 注意
>需要注意的是，一个图的邻接矩阵是唯一的，因此`基于邻接矩阵的 BFS 或 DFS 遍历序列也是唯一的`。

>而图的邻接表不是唯一的，边的输入顺序不同，正序或逆序建表都会影响邻接表的邻接点顺序，因此`基于邻接表的 BFS 或 DFS 遍历序列不是唯一的`。
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>图的遍历_深度优先遍历
