<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 一、概述
>`边集数组通过数组存储每条边的起点和终点。`
>
>如果是网，则再增加一个权值域。

## 二、边集数组的实现
我们以存储网为例。

以下为网的边集数组数据结构定义。


<font color=#999AAA >c++代码如下（示例）：

```cpp
struct Edge {
    int u;
    int v;
    int w;
} e[N * N];
```

<font color=#999AAA >java代码如下（示例）：

```java
private static class Edge {
    int u;
    int v;
    int w;
}

private static Edge e[] = new Edge[N * N];
```

### 1. 图解
以边集数组方式存储下图网，如图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/448bfcad640a407da4b087dd17918d8e.png)

### 2. 代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateGraph() {
    VexType u, v;
    int w;
    while (m--) {
        cin >> u >> v >> w;//输入相关联的节点和其权
        int i = LocateVex(u);//查找节点在节点数组的位置，不存在返回 -1
        int j = LocateVex(v);
        if (i != -1 && j != -1) {
            add(i, j, w);//将其添加到边集数组
        } else {
            cout << "错误！" << endl;
            m++;
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
private static void createGraph() {
    String u, v;
    int w;
  	while (m-- > 0) {
        u = sc.next();
        v = sc.next();
        w = sc.nextInt();
        int i = locateVex(u);
        int j = locateVex(v);
        if (i != -1 && j != -1) {
            add(i, j, w);
        } else {
            System.out.println("错误");
            m++;
        }
    }
}
```

## 三、完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
#include <cstring>

using namespace std;
typedef char VexType;
const int N = 1000;
int n, m, cnt;
VexType vex[N];

struct Edge {
    int u;
    int v;
    int w;
} e[N * N];

void Init() {
    memset(e, -1, sizeof e);
    memset(vex, -1, sizeof vex);
    cnt = 0;
}

int LocateVex(char x) {
    for (int i = 0; i < n; i++) {
        if (vex[i] == x) {
            return i;
        }
    }
    return -1;
}

void add(int i, int j, int w) {
    e[cnt].u = i;
    e[cnt].v = j;
    e[cnt++].w = w;
}

void CreateGraph() {
    VexType u, v;
    int w;
    while (m--) {
        cin >> u >> v >> w;
        int i = LocateVex(u);
        int j = LocateVex(v);
        if (i != -1 && j != -1) {
            add(i, j, w);
        } else {
            cout << "错误！" << endl;
            m++;
        }
    }
}

void Print() {
    cout << "边集数组如下：" << endl;
    for (int i = 0; i < cnt; i++) {
        cout << e[i].u << " " << e[i].v << " " << e[i].w << endl;
    }
}

int main() {
    Init();
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> vex[i];
    }
    CreateGraph();
    Print();

    return 0;
}
/*
4 6
A B C D
A B 3
A C 5
B C 2
C B 6
C D 2
D B 7
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
package E;

import java.util.Arrays;
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);
    private static final int N = 1000;
    private static int n, m, cnt;
    private static String vex[] = new String[N];

    private static class Edge {
        int u;
        int v;
        int w;
    }

    private static Edge e[] = new Edge[N * N];

    private static void init() {
        Arrays.fill(vex, -1);
        Arrays.fill(e, -1);
        cnt = 0;
    }

    private static int locateVex(String x) {
        for (int i = 0; i < n; i++) {
            if (vex[i].equals(x)) {
                return i;
            }
        }
        return -1;
    }

    private static void add(int i, int j, int w) {
        e[cnt] = new Edge();
        e[cnt].u = i;
        e[cnt].v = j;
        e[cnt++].w = w;
    }

    private static void createGraph() {
        String u, v;
        int w;
        while (m-- > 0) {
            u = sc.next();
            v = sc.next();
            w = sc.nextInt();
            int i = locateVex(u);
            int j = locateVex(v);
            if (i != -1 && j != -1) {
                add(i, j, w);
            } else {
                System.out.println("错误");
                m++;
            }
        }
    }

    private static void print() {
        System.out.println("边集数组如下：");
        for (int i = 0; i < cnt; i++) {
            System.out.println(e[i].u + " " + e[i].v + " " + e[i].w);
        }
    }

    public static void main(String[] args) {
        n = sc.nextInt();
        m = sc.nextInt();
        for (int i = 0; i < n; i++) {
            vex[i] = sc.next();
        }

        createGraph();
        print();

    }
}
/*
4 6
A B C D
A B 3
A C 5
B C 2
C B 6
C D 2
D B 7
 */
```

## 四、总结
### 1. 优点
1. 可以对边按权值排序；
2. 方便对边进行处理。

### 2. 缺点
1. 不便于判断两点之间是否右边；
2. 不便于访问所有邻接点；
3. 不便于计算各顶点的度。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

开学以后，日更有点不现实，事情有点多。

所以我决定，内容简单的就争取日更；内容多的，为了照顾那些赏脸看我文章的初学者，为他们多留出两天时间，好好整理、记忆，hh。

<font color=#999AAA >下期预告：</font>图的存储_链式前向星
