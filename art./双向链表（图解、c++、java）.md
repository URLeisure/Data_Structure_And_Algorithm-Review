

<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

# 双向链表的存储方式（图解）
学会了单链表，双向链表就很好理解。

<font color=#CC3300>**相对于单链表，双向链表只是又多了一个指针域，来存储上一个节点的地址，达到可以向前操作的效果。**</font>

![双向链表](https://img-blog.csdnimg.cn/07d71617b9084059b8135651389f105f.png)

## 双向链表的基本操作
<font color=#999AAA >因为双向链表与单链表的取值，查找代码相同，这里就不再写了。

首先定义一个结构体（内部类），包含数据域与两个指针域。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct DulNode{
    int data;
    DulNode *prior,*next;
}*DulList;
```

<font color=#999AAA >java代码如下（示例）：

```java
public class DulNode{
    int data;
    DulNode prior,next;
}
```

### 1.初始化
双向链表的初始化是指构建一个空表。先创建一个头节点，不存储数据，然后令其前后两个指针域为空（判断终止时会用到）。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Init(DulList &L){
    L = new DulNode;
    if(!L){
        return false;
    }
    L->prior = L->next = NULL;
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public DulNode init(){
    DulNode dulNode = new DulNode();
    dulNode.next = dulNode.prior = null;
    return dulNode;
}
```

### 2.创建
创建双向链表也可以用头插法和尾插法。

我们以头插法为例，尾插法与头插法改法相似，不再赘述。

![头插法](https://img-blog.csdnimg.cn/3ac3398073ac4343b1052ef3ee7f220b.png)


<font color=#999AAA >c++代码如下（示例）：

```cpp
void Create_H(DulList &L,int n){//n元素个数
    DulList s;
    while(n--){
        s = new DulNode;
        cin>>s->data;
        if(L->next){//判断头节点后有没有元素
            L->next->prior = s;//如果没有元素，就没有L->next->prior，会出bug
        }
        s->next = L->next;
        s->prior = L;
        L->next = s;
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public void create_H(DulNode L,int n){
    DulNode s;
    Scanner sc = new Scanner(System.in);
   	while(n-- > 0){
        s = new DulNode();
        s.data = sc.nextInt();
        if(L.next != null){
            L.next.prior = s;
        }
        s.next = L.next;
        s.prior = L;
        L.next = s;
    }
}
```



### 3.插入
                                                                         
![插入](https://img-blog.csdnimg.cn/aa22a3105d0c4c0d944cb1d332e47b56.png)
                                                                         
②、③位置不能颠倒。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool Insert(DulList &L,int i,int e){
    DulList s,p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i-1){
        return false;
    }
    s = new DulNode;
    s->data = e;
    if(p->next){
        p->next->prior = s;//如果p->next为空，则不存在p->next->prior，会出bug
    }
    s->next = p->next;
    s->prior = p;
    p->next = s;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public boolean insert(DulNode L,int i,int e){
    DulNode s,p = L;
    int j = 0;
    while(p!=null && j<i-1){
        p = p.next;
        j++;
    }
    if(p==null || j>i-1){
        return false;
    }
    s = new DulNode();
    s.data = e;
    if(p.next != null){
        p.next.prior = s;
    }
    s.next = p.next;
    s.prior = p;
    p.next = s;
    return true;
}
```

### 4.删除

删除一个节点，就是把这个节点跳过去。（可以不找 i-1 个，找第i个也行）
                                                                         
![删除](https://img-blog.csdnimg.cn/30717de8a5364840a0454326f6730731.png)

<font color=#999AAA >c++代码如下（示例）找第i-1个：

```cpp
bool Delete(DulList &L,int i,int &e){
    DulList q,p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i){
        return false;
    }
    q = p->next;
    if(q->next){
        q->next->prior = p;//如果p->next为空，则不存在p->next->prior，会出bug
    }
    p->next = q->next;//跨过q
    e = q->data;
    delete q;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）找第i个：

```java
public int delete(DulNode L,int i){
    DulNode p = L.next;
    int j = 1;
    while(p!=null && j<i){
        p = p.next;
        j++;
    }
    if(p==null || j>i){
        return -1;
    }
    if(p.next != null){
        p.next.prior = p.prior;
    }
    p.prior.next = p.next;
    return p.data;
}
```

### 5.打印
打印非常简单，直接上代码。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Print(DulList L) {
    DulList p;
    p = L->next;
    while (p) {
        cout << p->data << " ";
        p = p->next;
    }
    cout << endl;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public void print(DulNode L){
    DulNode p = L.next;
    while(p != null){
        System.out.print(p.data+" ");
        p = p.next;
    }
    System.out.println();
}
```

### 6.释放内存
c++需要手动释放内存，而java不需要。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Destroy(DulList &L){
    DulList tmp;
    for(DulList cur = L->next;cur != NULL;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}
```

### 7.完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;

typedef struct DulNode{
    int data;
    DulNode *prior,*next;
}*DulList;

bool Init(DulList &L){
    L = new DulNode;
    if(!L){
        return false;
    }
    L->prior = L->next = NULL;
    return true;
}

void Create_H(DulList &L,int n){
    DulList s;
    while(n--){
        s = new DulNode;
        cin>>s->data;
        if(L->next){
            L->next->prior = s;
        }
        s->next = L->next;
        s->prior = L;
        L->next = s;
    }
}

bool Insert(DulList &L,int i,int e){
    DulList s,p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i-1){
        return false;
    }
    s = new DulNode;
    s->data = e;

    if(p->next){
        p->next->prior = s;
    }
    s->next = p->next;
    s->prior = p;
    p->next = s;
    return true;
}

bool Delete(DulList &L,int i,int &e){
    DulList q,p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i){
        return false;
    }
    q = p->next;
    if(q->next){
        q->next->prior = p;
    }
    p->next = q->next;
    e = q->data;
    delete q;
    return true;
}

void Print(DulList L) {
    DulList p;
    p = L->next;
    while (p) {
        cout << p->data << " ";
        p = p->next;
    }
    cout << endl;
}

void Destroy(DulList &L){
    DulList tmp;
    for(DulList cur = L->next;cur != NULL;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}

int main(){
    DulList L;
    int choose=-1;
    int i,e,x,n;
    if(Init(L)){
        cout<<"初始化成功！"<<endl;
    }else{
        cout<<"初始化失败，请检查代码！"<<endl;
        return 0;
    }
    cout<<"输入元素个数："<<endl;
    cin>>n;
    cout<<"依次输入要插入的元素"<<endl;
    Create_H(L,n);
    cout << "双向链表表创建成功！"<<endl;

    cout << "1 .插入\n";
    cout << "2 .删除\n";
    cout << "3 .输出\n";
    cout << "0 .退出\n";
    while(choose != 0){
        cout<<"请选择！"<<endl;
        cin>>choose;
        switch (choose) {
            case 1:
                cout << "请输入要插入的位置和要插入的元素"<<endl;
                cin>>i>>e;
                if(Insert(L,i,e)){
                    cout << "插入成功！"<<endl;
                }else{
                    cout << "插入失败！"<<endl;
                }
                break;
            case 2:
                cout << "请输入要删除的位置i"<<endl;
                cin>>i;
                if(Delete(L,i,e)){
                    cout << "成功删除"<<e<<endl;
                }else{
                    cout<<"删除失败！"<<endl;
                }
                break;
            case 3:
                Print(L);
                break;
            case 0:
                cout << "成功退出"<<endl;
                break;
        }
    }
    Destroy(L);
    return 0;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public class D {

    public class DulNode{
        int data;
        DulNode prior,next;
    }

    public DulNode init(){
        DulNode dulNode = new DulNode();
        dulNode.next = dulNode.prior = null;
        return dulNode;
    }

    public void create_H(DulNode L,int n){
        DulNode s;
        Scanner sc = new Scanner(System.in);
        while(n-- > 0){
            s = new DulNode();
            s.data = sc.nextInt();
            if(L.next != null){
                L.next.prior = s;
            }
            s.next = L.next;
            s.prior = L;
            L.next = s;
        }
    }

    public boolean insert(DulNode L,int i,int e){
        DulNode s,p = L;
        int j = 0;
        while(p!=null && j<i-1){
            p = p.next;
            j++;
        }
        if(p==null || j>i-1){
            return false;
        }
        s = new DulNode();
        s.data = e;
        if(p.next != null){
            p.next.prior = s;
        }
        s.next = p.next;
        s.prior = p;
        p.next = s;
        return true;
    }

    public int delete(DulNode L,int i){
        DulNode p = L.next;
        int j = 1;
        while(p!=null && j<i){
            p = p.next;
            j++;
        }
        if(p==null || j>i){
            return -1;
        }
        if(p.next != null){
            p.next.prior = p.prior;
        }
        p.prior.next = p.next;
        return p.data;
    }

    public void print(DulNode L){
        DulNode p = L.next;
        while(p != null){
            System.out.print(p.data+" ");
            p = p.next;
        }
        System.out.println();
    }
    public static void main(String[] args) {
        D d = new D();
        DulNode L;
        int n,e,i;
        Scanner sc = new Scanner(System.in);
        L = d.init();
        System.out.println("双向链表初始化成功！");
        System.out.println("输入元素个数：");
        n = sc.nextInt();
        System.out.println("依次输入要插入的元素");
        d.create_H(L,n);
        System.out.println("创建成功！");
        System.out.print("1.插入\n2.删除\n3.打印\n0.退出\n");
        int choose = -1;
        while(choose != 0){
            choose = sc.nextInt();
            switch(choose){
                case 1:
                    System.out.println("请输入要插入的位置和元素");
                    i = sc.nextInt();
                    e = sc.nextInt();
                    if(d.insert(L,i,e)){
                        System.out.println("插入成功！");
                    }else
                        System.out.println("插入失败！");
                    break;
                case 2:
                    System.out.println("请输入要删除的位置");
                    i = sc.nextInt();
                    System.out.println("已删除"+d.delete(L,i));
                    break;
                case 3:
                    d.print(L);
                    break;
                case 0:
                    break;
            }
        }
    }
}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结

对于学会了单链表的人来说，双向链表看一看就差不多了。

单链表和双链表的区别也就是，一个不可以向前操作，另一个可以向前操作。

同时记着在插入和删除时修改prior指针节点。

<font color=#999AAA >下期预告：</font>循环链表
