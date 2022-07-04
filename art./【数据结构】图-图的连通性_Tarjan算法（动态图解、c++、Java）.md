<font color=#CC3300>公众号：</font>URLeisure 的复习仓库
<font color=#8B6914 >公众号二维码见文末</font>

<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


# 概述
* Robert Tarjan 以在数据结构和图论上的开创性工作而闻名，他的一些著名算法包括 Tarjan 最近公共祖先离线算法、Tarjan 强连通分量算法及 Link-Cut-Trees 算法等。
* 其中 Hopcroft-Tarjan 平面嵌入算法是第 1 个线性时间平面算法。
* Robert Tarjan 也开创了重要的数据结构，例如斐波那契堆和 Splay 树，另一项重大贡献是分析了并查集。
# 前情提要
* 学习Tarjan 算法需要了解时间戳和追溯点的概念。
* **`时间戳：`** dfn[u] 表示节点 u 深度优先遍历的序号。
* **`追溯点：`** low[u] 表示节点 u 或 u 的子孙能通过非父子边追溯到 dfn 最小的节点序号，即回到最早的过去。

**`例如：`**

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c1ce8b127764e8da66fbe86aebde244.png)

* 在[深度优先搜索](https://blog.csdn.net/weixin_50564032/article/details/123862349?spm=1001.2014.3001.5502)中，每个节点的时间戳和追溯点的求解过程如下。
* 初始化时，**`dfn[u] = low[u] = ++num`**，如果该节点的邻接点未被访问，则一直进行深度优先遍历
$$1→2→3→5→6→4$$
* 当到达 4 时，4 的邻接点 1 已被访问，且 1 不是 4 的父节点（4的父节点是 6）。
* 此时开始回溯，且根据追溯点定义，更新 low
	 1. 4 能回到最早的节点是 1（dfn = 1），因此 **`low[4] = min(low[4], dfn[1]) = 1`** ；
	 2. 回到上一个节点 6，**`low[6] = min(low[6], low[4]) = 1`** ；
	 3. ...
	 4. 回到上个节点 3，发现其还有邻接点 7 没有访问，因此 **`low[7] = dfn[7] = ++num = 7`**。因为 7 没有子节点，所以 low[7] 不用更新。
	5. ...

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f8e0f557e5e47ca88b23610b4850296.png)
# 无向图的桥判定法则
## 判定
* 无向边 x-y 是桥，当且仅当在搜索树上存在 x 的一个子节点 y 时，满足 **`low[y] > dfn[x]`**。
* **`若孩子的 low 值比自己的 dfn 值大`**，则从该节点到这个孩子的边为桥。

图中，3-4 即为桥。
![在这里插入图片描述](https://img-blog.csdnimg.cn/90faadb54d5d4a29bdecdf320e053b2e.png)
## 代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include <cstring>

using namespace std;
const int N = 1e4;

struct Edge {
    int to;
    int next;
} e[N << 1];
int low[N], dfn[N], head[N];
int cnt, num;

void Init() {
    memset(low, 0, sizeof low);
    memset(dfn, 0, sizeof dfn);
    memset(head, -1, sizeof head);
    cnt = num = 0;
}

void add(int u, int v) {
    e[cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void Tarjan(int u, int fa) {
    low[u] = dfn[u] = ++num;
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (v == fa) {//无向图，防止重复判断
            continue;
        }
        if (!dfn[v]) {//u 的下个节点没有访问过
            Tarjan(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > dfn[u]) {
                cout << u << "-" << v << "是桥" << endl;
            }
        } else {//u 的下一个节点访问过
            low[u] = min(low[u], dfn[v]);
        }
    }
}

int main() {
    int m;
    cin >> m;
    Init();
    while (m--) {
        int u, v;
        cin >> u >> v;
        add(u, v);
        add(v, u);
    }
    Tarjan(1, 0);
    return 0;
}
/*
7
1 2
2 3
3 7
3 5
5 6
6 4
4 1
*/
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class A {
    public static final int N = 10000;
    public static Scanner sc = new Scanner(System.in);

    public static class edge {
        int to;
        int next;
    }

    public static edge[] e = new edge[N << 1];
    public static int[] low = new int[N];
    public static int[] dfn = new int[N];
    public static int[] head = new int[N];
    public static int num, cnt;

    public static void init() {
        Arrays.fill(low, 0);
        Arrays.fill(dfn, 0);
        Arrays.fill(head, -1);
        num = cnt = 0;
    }

    public static void add(int u, int v) {
        e[cnt] = new edge();
        e[cnt].to = v;
        e[cnt].next = head[u];
        head[u] = cnt++;
    }

    public static void Tarjan(int u, int fa) {
        low[u] = dfn[u] = ++num;
        for (int i = head[u]; i != -1; i = e[i].next) {
            int v = e[i].to;
            if (v == fa) {
                continue;
            }
            if (dfn[v] == 0) {
                Tarjan(v, u);
                low[u] = Math.min(low[u], low[v]);
                if (low[v] > dfn[u]) {
                    System.out.println(u + "-" + v + "是桥");
                }
            } else {
                low[u] = Math.min(low[u], dfn[v]);
            }
        }
    }

    public static void main(String[] args) {
        int m;
        m = sc.nextInt();
        init();
        while (m-- > 0) {
            int u, v;
            u = sc.nextInt();
            v = sc.nextInt();
            add(u, v);
            add(v, u);
        }
        Tarjan(1, 0);
    }
}
/*
7
1 2
2 3
3 7
3 5
5 6
6 4
4 1
*/
```

# 无向图割点判定法则
## 判定
1. **`若 x 不是根节点`**，当且仅当在搜索树上存在 x 的一个子节点 y，**`满足 low[y] > dfn[x]`**，则 x 是割点。
2. **`若 x 是根节点，且满足 low[y] == dfn[x]`**，则 x 是割点。

**`当且仅当在搜索树上至少存在两个子节点，都满足该条件。`**
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b65253d27594a238616868887a5efcb.png)
**上图，第三个图中 1 不是割点，虽然 low[2] == dfn[1] 且 low[4] == dfn[1] 但是，4 不是 1 的孩子。**

## 代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include<cstring>

using namespace std;

const int N = 1e4;

struct Edge {
    int to;
    int next;
} e[N << 1];

int low[N], dfn[N], head[N];
int cnt, num, root;

void init() {
    memset(low, 0, sizeof low);
    memset(dfn, 0, sizeof dfn);
    memset(head, -1, sizeof head);
    cnt = num = 0;
}

void add(int u, int v) {
    e[cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void Tarjan(int u, int fa) {
    low[u] = dfn[u] = ++num;
    int count = 0;
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (v == fa) {
            continue;
        }
        if (!dfn[v]) {
            Tarjan(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] >= dfn[u]) {
                count++;//判断是跟的话至少要有两个孩子满足条件
                if (u != root || count > 1) {
                    cout << u << "是割点" << endl;
                }
            }
        } else {
            low[u] = min(low[u], dfn[v]);
        }
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    init();
    while (m--) {
        int u, v;
        cin >> u >> v;
        add(u, v);
        add(v, u);
    }
    for (int i = 1; i <= n; i++) {
        root = i;
        Tarjan(i, 0);
    }
    return 0;
}
/*
8 7
1 2
2 3
3 7
3 5
5 6
6 4
4 1
*/
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class A {
    public static final int N = 10000;
    public static Scanner sc = new Scanner(System.in);

    public static class edge {
        int to;
        int next;
    }

    public static edge[] e = new edge[N << 1];
    public static int[] low = new int[N];
    public static int[] dfn = new int[N];
    public static int[] head = new int[N];
    public static int num, cnt, root;

    public static void init() {
        Arrays.fill(low, 0);
        Arrays.fill(dfn, 0);
        Arrays.fill(head, -1);
        num = cnt = 0;
    }

    public static void add(int u, int v) {
        e[cnt] = new edge();
        e[cnt].to = v;
        e[cnt].next = head[u];
        head[u] = cnt++;
    }

    public static void Tarjan(int u, int fa) {
        low[u] = dfn[u] = ++num;
        int count = 0;
        for (int i = head[u]; i != -1; i = e[i].next) {
            int v = e[i].to;
            if (v == fa) {
                continue;
            }
            if (dfn[v] == 0) {
                Tarjan(v, u);
                low[u] = Math.min(low[u], low[v]);
                if (low[v] >= dfn[u]) {
                    count++;
                    if (u != root || count > 1) {
                        System.out.println(u + "是割点");
                    }
                }
            } else {
                low[u] = Math.min(low[u], dfn[v]);
            }
        }
    }

    public static void main(String[] args) {
        int n, m;
        n = sc.nextInt();
        m = sc.nextInt();
        init();
        while (m-- > 0) {
            int u, v;
            u = sc.nextInt();
            v = sc.nextInt();
            add(u, v);
            add(v, u);
        }
        for (int i = 1; i <= n; i++) {
            root = i;
            Tarjan(i, 0);
        }
    }
}
/*
8 7
1 2
2 3
3 7
3 5
5 6
6 4
4 1
*/
```

# 有向图的强连通分量
## 算法步骤
* [深度优先遍历](https://blog.csdn.net/weixin_50564032/article/details/123862349?spm=1001.2014.3001.5502)节点，在第一次访问节点 x 时，将 x 入栈，且 **`dfn[x] = low[x] = ++num`**；
* 遍历 x 的所有邻接点 y,
	1.  若 y 没有被访问，则递归访问 y，返回时更新 **`low[x] = min(low[x], low[y])`**；
	2. 若 y 已被访问且在栈中，则令 **`low[x] = min(low[x], dfny])`**
* 在 x 回溯之前，如果判断 low[x] == dfn[x]，则从栈中不断弹出节点，直到 x 出栈时停止。弹出的节点集合就是一个强连通分量。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3a575cdf82ef4ec9a68ffb72b417de33.gif)

## 代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include<cstring>
#include<stack>

using namespace std;

const int N = 1e4;

struct Edge {
    int to;
    int next;
} e[N << 1];

int low[N], dfn[N], head[N];
int cnt, num;
bool vis[N];
stack<int> s;

void init() {
    memset(low, 0, sizeof low);
    memset(dfn, 0, sizeof dfn);
    memset(head, -1, sizeof head);
    memset(vis, false, sizeof vis);
    cnt = num = 0;
}

void add(int u, int v) {
    e[cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void Tarjan(int u) {
    low[u] = dfn[u] = ++num;
    vis[u] = true;
    s.push(u);
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (!dfn[v]) {
            Tarjan(v);
            low[u] = min(low[u], low[v]);
        } else if (vis[u]) {
            low[u] = min(low[u], dfn[v]);
        }
    }

    if (low[u] == dfn[u]) {
        int v;
        do {
            v = s.top();
            s.pop();
            cout << v << " ";
            vis[u] = false;
        } while (v != u);
        cout << endl;
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    init();
    while (m--) {
        int u, v;
        cin >> u >> v;
        add(u, v);
    }
    for (int i = 1; i <= n; i++) {
        if (!dfn[i]) {
            Tarjan(i);
        }
    }
    return 0;
}
/*
5 8
4 5
4 3
3 1
2 5
2 3
2 4
1 4
1 2
*/
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Scanner;

/**
 * @author URLeisure
 * @create 2022/7/4 15:14
 * @Description 连通分量
 */
public class L {
    public static final int N = 10000;
    public static Scanner sc = new Scanner(System.in);

    public static class edge {
        int to;
        int next;
    }

    public static edge[] e = new edge[N << 1];
    public static int[] low = new int[N];
    public static int[] dfn = new int[N];
    public static int[] head = new int[N];
    public static int num, cnt;
    public static LinkedList<Integer> s = new LinkedList<>();
    public static boolean[] vis = new boolean[N];

    public static void init() {
        Arrays.fill(low, 0);
        Arrays.fill(dfn, 0);
        Arrays.fill(head, -1);
        Arrays.fill(vis, false);

        num = cnt = 0;
    }

    public static void add(int u, int v) {
        e[cnt] = new edge();
        e[cnt].to = v;
        e[cnt].next = head[u];
        head[u] = cnt++;
    }

    public static void Tarjan(int u) {
        low[u] = dfn[u] = ++num;
        vis[u] = true;
        s.push(u);
        for (int i = head[u]; i != -1; i = e[i].next) {
            int v = e[i].to;

            if (dfn[v] == 0) {
                Tarjan(v);
                low[u] = Math.min(low[u], low[v]);
            } else if (vis[v]) {
                low[u] = Math.min(low[u], dfn[v]);
            }
        }
        if (low[u] == dfn[u]) {
            int v;
            do {
                v = s.pop();
                System.out.print(v + " ");
                vis[v] = false;
            } while (v != u);
            System.out.println();
        }
    }

    public static void main(String[] args) {
        int n, m;
        n = sc.nextInt();
        m = sc.nextInt();
        init();
        while (m-- > 0) {
            int u, v;
            u = sc.nextInt();
            v = sc.nextInt();
            add(u, v);
        }
        for (int i = 1; i <= n; i++) {
            if (dfn[i] == 0) {
                Tarjan(i);
            }
        }
    }
}
/*
5 8
4 5
4 3
3 1
2 5
2 3
2 4
1 4
1 2
*/
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">
<font color=#FF9900 >关注公众号，感受不同的阅读体验</font>

![请添加图片描述](https://img-blog.csdnimg.cn/96b40e5bd90145ef9effa9742de88115.jpeg)
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>最短路径_Dijkstra算法
