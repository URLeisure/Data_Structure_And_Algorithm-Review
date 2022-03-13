<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## 一、概述
>链式前向星采用了一种静态链表存储方式，将`边集数组`和`邻接表`相结合，可以快速访问一个节点的所有邻接点，在算法竞赛中被广泛使用。

## 二、链式前向星的实现

链式前向星有如下两种存储结构。

1. `边集数组：`edge[ ]，edge[i] 表示第 i 条边；
2. `头节点数组：`head[ ]，head[i] 表示存储以 i 为起点的第 1 条边的下标（edge[ ] 中的下标）。

<font color=#999AAA >c++代码如下（示例）：

```cpp
struct Edge {
    int to;
    int w;
    int next;
} edge[Maxn * Maxn];
int head[Maxn];
```

<font color=#999AAA >java代码如下（示例）：

```java
private static class Edge {
    int to;
    int w;
    int next;
}
private static Edge[] edge = new Edge[maxn * maxn];
private static int[] head = new int[maxn];
```

### 1. 图解
每条边的结构都如下图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/be8cebe287fe49c383ff5a91901eb5bc.png)

其中，
* edge[i].to 存储 head[i] 的一个邻接点
* edge[i].w 存储这条边的权
* edge[i].next 存储 head[i] 的另一个邻接点索引

以下图有向图为例，创建链式前向星如图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/25e9c643bb30437cb0b19d6a97bfd899.png)

`当 head[i] 或 edge[i].next 为 -1 时，表示没有邻接点`。

* head[ ] 的索引 `u` 表示第几个节点
* edge[ ] 存储边
* head[u] 存储 edge[ ] 中与该节点相关的边的索引（head[u] 存储 edge[ ] 的索引 `i`）
* edge[i].next 存储与 u 相关的 edge[ ] 中其他边的索引

![在这里插入图片描述](https://img-blog.csdnimg.cn/e8b1cb11781d4a218669e1d55ecb2294.png)

总结一下，`俩数组 head[] 和 egde[].next 存的都是 edge[] 的索引`。

### 2. 特性
从图中不难看出：
1. 和邻接表一样，因为链式前向星采用头插法进行链接，所以边的输入顺序不同，创建的链式前向星也不同。
2. 对于无向图，每输入一条边，都需要添加两条边，互为反向边。(完整代码中会给出示例，c++ 第45行，java 第51行)
3. 链式前向星具有边集数组和邻接表的功能，属于静态链表，不需要频繁地创建节点，应用起来十分灵活。


### 3. 代码
”头插法“完成添加操作。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void add(int u, int v, int w) {
    edge[cnt].to = v;//cnt 为 edge 索引
    edge[cnt].w = w;
    edge[cnt].next = head[u];//类似头插法，先取得头节点的信息，再更新头节点
    head[u] = cnt++;//更新头节点
}
```

<font color=#999AAA >java代码如下（示例）：

```java
private static void add(int u, int v, int w) {
    edge[cnt] = new Edge();
    edge[cnt].to = v;
    edge[cnt].w = w;
    edge[cnt].next = head[u];
    head[u] = cnt++;
}
```

## 三、完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include <cstring>

using namespace std;
#define Maxn 100
int n, m, cnt;

struct Edge {
    int to;
    int w;
    int next;
} edge[Maxn * Maxn];
int head[Maxn];

void init() {
    memset(head, -1, sizeof head);
    memset(edge, 0, sizeof edge);
    cnt = 0;
}

void add(int u, int v, int w) {
    edge[cnt].to = v;
    edge[cnt].w = w;
    edge[cnt].next = head[u];
    head[u] = cnt++;
}

void print() {
    for (int u = 1; u <= n; u++) {
        cout << u << " ";
        for (int i = head[u]; ~i; i = edge[i].next) {//~i 就是 i != -1
            cout << "[" << edge[i].to << " " << edge[i].w << " " << edge[i].next << "]";
        }
        puts("");
    }
}

int main() {
    init();
    cin >> n >> m;
    int u, v, w;
    for (int i = 0; i < m; i++) {
        cin >> u >> v >> w;
        add(u, v, w);
        //add(v,u,w);
    }
    print();
    return 0;
}
/*
4 5
1 2 6
1 4 2
3 1 5
3 4 8
4 2 3
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Arrays;
import java.util.Scanner;

public class  {
    private static Scanner sc = new Scanner(System.in);
    private static final int maxn = 100;
    private static int n, m, cnt;

    private static class Edge {
        int to;
        int w;
        int next;
    }

    private static Edge[] edge = new Edge[maxn * maxn];
    private static int[] head = new int[maxn];

    private static void init() {
        Arrays.fill(head, -1);
        cnt = 0;
    }

    private static void add(int u, int v, int w) {
        edge[cnt] = new Edge();
        edge[cnt].to = v;
        edge[cnt].w = w;
        edge[cnt].next = head[u];
        head[u] = cnt++;
    }

    private static void print() {
        for (int u = 1; u <= n; u++) {
            System.out.print(u + " ");
            for (int i = head[u]; i != -1; i = edge[i].next) {
                System.out.print("[" + edge[i].to + " " + edge[i].w + " " + edge[i].next + "]");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        init();
        n = sc.nextInt();
        m = sc.nextInt();
        int u, v, w;
        for (int i = 0; i < m; i++) {
            u = sc.nextInt();
            v = sc.nextInt();
            w = sc.nextInt();
            add(u, v, w);
            //add(v,u,w);
        }
        print();
    }
}
/*
4 5
1 2 6
1 4 2
3 1 5
3 4 8
4 2 3
 */
```
## 总结
链式前向星中 head[ ] 和 edge[ ].next 存储的`都是 edge[] 的索引！`

是 edge[ ] 的索引！

edge[ ] 的索引！

的索引！

索引！
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>图的遍历_广度优先遍历
