


<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 什么是链表？

>链表是线性表的链式存储方式。逻辑上相邻的数据在计算机内的存储位置不一定相邻。
>链表由节点组成，每个节点都包含下一个节点的指针

## 单链表的存储方式（图解）
![指针](https://img-blog.csdnimg.cn/0d71ee4e32204b508487d0623ac00e9b.png)

给每个元素附加一个指针域，指向下一个元素的存储位置。

由图可以看出，每个节点包含两个域：<font color=#CC3300>**数据域**</font>和<font color=#CC3300>**指针域**</font>。

<font color=#CC3300>**数据域存储元素，指针域存储下一个节点的地址**，</font>因此指针指向的类型也是节点类型。

每个指针都指向下一个节点，都是朝一个方向的，这样的链表称为单向链表或单链表。

脑里有图的话，链表学起来还是很简单的。
## 单链表的基本操作
首先定义一个结构体（内部类），包含数据域与指针域。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct LNode{
    int date;
    LNode *next;
}*LinkList;
```

<font color=#999AAA >java代码如下（示例）：

```java
private class LNode{
 	int date;
	LNode next;
}
```

### 1.初始化
单链表的初始化是指构建一个空表。先创建一个头节点，不存储数据，然后令其指针域为空（判断终止时会用到）。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool InitList(LinkList &L){
    L = new LNode;//生成新节点
    if(!L){//鲁棒
        return false;
    }
    L->date = 0;
    L->next = NULL;//下一个节点为空
    return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public boolean init(){
	lnode = new LNode(
	if(lnode==null){
     	return false;
   	}	
    lnode.next = null;
    return true;
}
```

### 2.创建
创建单链表分为<font color=#CC3300>**头插法**</font>和<font color=#CC3300>**尾插法**</font>两种。

<font color=#CC3300>**头插法**</font>是指每次把新节点插到头节点之后，其创建的单链表和数据输入顺序正好相反，因此也称为<font color=#CC3300>**逆序建表**</font>。

![头插法](https://img-blog.csdnimg.cn/aa1dc3d0cd9647529ef202c6fcdd2d9f.png)
                                                                         
头插法头插法，就是新指针插到头指针后面。也就是上次插入元素的前面。

很明显可以看出，输出顺序与输入顺序相反。有一种栈（后进先出）的感觉，后面栈链的实现也确实使用的这种方法。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateList_H(LinkList &L,int n){
    LinkList s;
    while(n--){
        s = new LNode;
        cin>>s->date;
        //新节点next替换头节点next
        s->next = L->next;
        L->next = s;
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public void create_H(int n){
	LNode s;
	while(n-- > 0){
		s = new LNode();
		s.date = input.nextInt();
		s.next = lnode.next;
        lnode.next = s;
    }
}
```


<font color=#CC3300>**尾插法**</font>是指每次把新节点链接到链表尾部，其创建的单链表和数据输入顺序一致，因此也称为<font color=#CC3300>**正序建表**</font>。
![在这里插入图片描述](https://img-blog.csdnimg.cn/87771986d3864f289467c08b0bd791f2.png)
                                                                         
尾插法，将新节点链接到尾节点（r）后面，非常的简单。

注意新节点的next与尾结点的更新。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateList_R(LinkList &L,int n){
    LinkList s;
    LinkList r = L;
    while(n--){
        s = new LNode;
        cin>>s->date;
        s->next = NULL;
        r->next = s;//节点连上
        r = s;//更新尾节点
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public void create_R(int n){
   	LNode s,r = lnode;
    while(n-- > 0){
      	s = new LNode();
        s.date = input.nextInt();
        s.next = null;
        r.next = s;
        r = s;
   	}
}
```


### 3.取值
单链表的取值不像顺序表那样可以随机访问任何一个元素，单链表只有头指针，各个节点的物理地址是不连续的。

要想找到第i个节点，就必须从第一个节点开始顺序向后找，一直找到第i个节点。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool GetElem(LinkList L,int i,int &e) {//i 查看第i个元素 e为此元素的值 
    LinkList p = L->next;
    int j = 1;
    while (p && j < i) {
        p = p->next;
        j++;
    }
    if (!p) {
        return false;
    }
    e = p->date;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int get(int i){
   	LNode p = lnode.next;
    int j = 1;
    while(p!=null && j<i){
  	   p = p.next;
       j++;
    }
    if(p==null && j>i){
       return -1;
    }
    return p.date;
}
```

### 4.查找
查找是否存在元素e，可以定义一个p指针，指向第一个元素节点，比较p指向节点的数据域是否等于e。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool LocateElem(LinkList L,int e){
    LinkList p = L->next;
    while(p && p->date!=e){
        p = p->next;
    }
    if(!p)//不存在e时，p == NULL
        return false;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int locate(int e){
  	LNode p = lnode.next;
    int i = 1;
    while(p!=null && p.date!=e){
    	  p = p.next;
          i++;
   	}
    if(p == null){
          return -1;
    }
    return i;
}
```

### 5.插入
![插入](https://img-blog.csdnimg.cn/33637d5498784c7d8adec40fd7f07323.png)
                                                                         
要在第i个节点前插入一个元素，则必须先找到第i-1个节点。

<font color=#CC3300>**因为单链表只有一个指针域，是向后操作的，无法向前操作。**</font>

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool ListInsert(LinkList &L,int i,int e){//第i个位置插入元素e
    LinkList p = L;
    int j = 0;
    while(p && j<i-1){//寻找前一个
        p = p->next;
        j++;
    }
    if(!p || j>i-1){//防止i超出范围（i<1 || i>length）
        return false;
    }
    LinkList s = new LNode;
    s->date = e;
    s->next = p->next;
    p->next = s;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public boolean insert(int i,int e){
  	LNode s,p = lnode;
    int j = 0;
    while(p!=null && j<i-1){
       	p = p.next;
        j++;
    }
    if(p==null || j>i-1){
        return false;
    }
    s = new LNode();
    s.date = e;
    s.next = p.next;
    p.next = s;
    return true;
}
```

### 6.删除


![删除](https://img-blog.csdnimg.cn/162f85f64f72463295fd1f5d43b2523b.png)
                                                                         
删除一个节点，实际上是把这个节点跳过去。

根据点链表向后操作的特性，要跳过第i个节点，就必须先找到第i-1个节点。

删除同时，c++需要释放内存。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool ListDelete(LinkList &L,int i,int &e){//删除第i个位置 记录删除的元素值e
    LinkList p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i-1){//防止i超出范围（i<1 || i>length）
        return false;
    }
    LinkList q = p->next;
    p->next = q->next;
    e = q->date;
    delete q;
    return true;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public int delete(int i){
    LNode q,p = lnode;
    int j = 0;
    while(p!=null && j<i-1){
      	p = p.next;
        j++;
   	}
    if(p==null || j>i-1){
        return -1;
    }
    q = p.next;
    p.next = q.next;
    return q.date;
}
```

### 7.打印
打印非常简单，直接上代码。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void PrintList(LinkList L){
    LinkList p = L->next;
    while(p){
        cout<<p->date<<" ";
        p = p->next;
    }
    cout<<endl;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public void print(){
   	LNode p = lnode.next;
    while(p != null){
     	System.out.print(p.date+" ");
        p = p.next;
    }
    System.out.println();
}
```

### 8.释放内存
c++需要手动释放内存，而java不需要。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void DestroyList(LinkList &L){
    LinkList tmp;
    for(LinkList cur = L->next;cur!=NULL;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}
```

### 9.完整代码
<font color=#999AAA >c++代码如下（示例）：

```cpp
#include <iostream>
using namespace std;

typedef struct LNode{
    int date;
    LNode *next;
}*LinkList;

bool InitList(LinkList &L){
    L = new LNode;
    if(!L){
        return false;
    }
    L->date = 0;
    L->next = NULL;
    return true;
}

void CreateList_H(LinkList &L,int n){
    LinkList s;
    while(n--){
        s = new LNode;
        cin>>s->date;
        s->next = L->next;
        L->next = s;
    }
}

void CreateList_R(LinkList &L,int n){
    LinkList s;
    LinkList r = L;
    while(n--){
        s = new LNode;
        cin>>s->date;
        s->next = NULL;
        r->next = s;
        r = s;
    }
}

bool GetElem(LinkList L,int i,int &e) {
    LinkList p = L->next;
    int j = 1;
    while (p && j < i) {
        p = p->next;
        j++;
    }
    if (!p) {
        return false;
    }
    e = p->date;
    return true;
}

bool LocateElem(LinkList L,int e){
    LinkList p = L->next;
    while(p && p->date!=e){
        p = p->next;
    }
    if(!p)
        return false;
    return true;
}

bool ListInsert(LinkList &L,int i,int e){
    LinkList p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i-1){
        return false;
    }
    LinkList s = new LNode;
    s->date = e;
    s->next = p->next;
    p->next = s;
    return true;
}

bool ListDelete(LinkList &L,int i,int &e){
    LinkList p = L;
    int j = 0;
    while(p && j<i-1){
        p = p->next;
        j++;
    }
    if(!p || j>i-1){
        return false;
    }
    LinkList q = p->next;
    p->next = q->next;
    e = q->date;
    delete q;
    return true;
}

void PrintList(LinkList L){
    LinkList p = L->next;
    while(p){
        cout<<p->date<<" ";
        p = p->next;
    }
    cout<<endl;
}

void DestroyList(LinkList &L){
    LinkList tmp;
    for(LinkList cur = L->next;cur!=NULL;){
        tmp = cur;
        cur = cur->next;
        delete tmp;
    }
    delete L;
}

int main() {
    LinkList L;
    int choose=-1;
    int i,e,x,n;
    if(InitList(L)){
        cout<<"初始化成功！"<<endl;
    }else{
        cout<<"初始化失败，请检查代码！"<<endl;
        return 0;
    }
    cout<<"1.头插法创建链表\n2.尾插法创建链表"<<endl;
    cin>>choose;
    cout<<"输入元素个数："<<endl;
    cin>>n;
    cout<<"依次输入要插入的元素"<<endl;
    if(choose==1){
        CreateList_H(L,n);
    }else if(choose==2){
        CreateList_R(L,n);
    }else{
        cout<<"输入错误！"<<endl;
        return 0;
    }

    cout << "单链表表创建成功！"<<endl;

    cout << "1 .取值\n";
    cout << "2 .查找\n";
    cout << "3 .插入\n";
    cout << "4 .删除\n";
    cout << "5 .输出\n";
    cout << "0 .退出\n";
    while(choose != 0){
        cout<<"请选择！";
        cin>>choose;
        switch (choose) {
            case 1:
                cout << "输入整形i，取第i个元素！"<<endl;
                cin>>i;
                if(GetElem(L,i,e)){
                    cout << "第"<<i<<"个元素是"<<e<<endl;
                }else{
                    cout << "取元素失败！"<<endl;
                }
                break;
            case 2:
                cout<<"输入要查找的元素"<<endl;
                cin>>x;
                if(LocateElem(L,x)){
                    cout << "查找成功！"<<endl;
                }else{
                    cout << "查找失败！"<<endl;
                }

                break;
            case 3:
                cout << "请输入要插入的位置和要插入的元素"<<endl;
                cin>>i>>e;
                if(ListInsert(L,i,e)){
                    cout << "插入成功！"<<endl;
                }else{
                    cout << "插入失败！"<<endl;
                }
                break;
            case 4:
                cout << "请输入要删除的位置i"<<endl;
                cin>>i;
                if(ListDelete(L,i,e)){
                    cout << "成功删除"<<e<<endl;
                }else{
                    cout<<"删除失败！"<<endl;
                }
                break;
            case 5:
                PrintList(L);
                break;
            case 0:
                cout << "成功退出"<<endl;
                break;
        }
    }
    DestroyList(L);
    return 0;
}
```
<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    private Scanner input = new Scanner(System.in);
    private LNode lnode;
    private class LNode{
        int date;
        LNode next;
    }

    public boolean init(){
        lnode = new LNode();
        if(lnode==null){
            return false;
        }
        lnode.next = null;
        return true;
    }

    public void create_H(int n){
        LNode s;
        while(n-- > 0){
            s = new LNode();
            s.date = input.nextInt();
            s.next = lnode.next;
            lnode.next = s;
        }
    }

    public void create_R(int n){
        LNode s,r = lnode;
        while(n-- > 0){
            s = new LNode();
            s.date = input.nextInt();
            s.next = null;
            r.next = s;
            r = s;
        }
    }

    public int get(int i){
        LNode p = lnode.next;
        int j = 1;
        while(p!=null && j<i){
            p = p.next;
            j++;
        }
        if(p==null && j>i){
            return -1;
        }
        return p.date;
    }

    public int locate(int e){
        LNode p = lnode.next;
        int i = 1;
        while(p!=null && p.date!=e){
            p = p.next;
            i++;
        }
        if(p == null){
            return -1;
        }
        return i;
    }

    public boolean insert(int i,int e){
        LNode s,p = lnode;
        int j = 0;
        while(p!=null && j<i-1){
            p = p.next;
            j++;
        }
        if(p==null || j>i-1){
            return false;
        }
        s = new LNode();
        s.date = e;
        s.next = p.next;
        p.next = s;
        return true;
    }

    public int delete(int i){
        LNode q,p = lnode;
        int j = 0;
        while(p!=null && j<i-1){
            p = p.next;
            j++;
        }
        if(p==null || j>i-1){
            return -1;
        }
        q = p.next;
        p.next = q.next;
        return q.date;
    }

    public void print(){
        LNode p = lnode.next;
        while(p != null){
            System.out.print(p.date+" ");
            p = p.next;
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        A a = new A();

        int e = 0,i =0,n;
        if (a.init()){
            System.out.println("1.头插法\n2.尾插法");
        }else{
            System.out.println("初始化失败，请检查程序");
            return ;
        }
        int choose = a.input.nextInt();
        System.out.println("请输入元素个数：");
        n = a.input.nextInt();
        System.out.println("请依次输入元素");
        switch (choose){
            case 1:
                a.create_H(n);
                break;
            case 2:
                a.create_R(n);
                break;
        }
        System.out.print("1. 取值\n2. 查找\n3. 插入\n4. 删除\n5. 输出\n0. 退出\n");
        choose = -1;
        while(choose != 0){
            choose = a.input.nextInt();
            switch (choose){
                case 1:
                    System.out.println("请输入要查看的位置");
                    i = a.input.nextInt();
                    if((e=a.get(i))!=-1){
                        System.out.println("第"+i+"个元素为"+e);
                    }else{
                        System.out.println("取值失败！");
                    }
                    break;
                case 2:
                    System.out.println("请输入要查看的元素");
                    e = a.input.nextInt();
                    if((i = a.locate(e))!=-1){
                        System.out.println(e+"是第"+i+"个元素！");
                    }else{
                        System.out.println("查找失败！");
                    }
                    break;
                case 3:
                    System.out.println("请输入要插入的位置和元素");
                    i = a.input.nextInt();
                    e = a.input.nextInt();
                    if(a.insert(i,e)){
                        System.out.println("插入成功！");
                    }else{
                        System.out.println("插入失败！");
                    }
                    break;
                case 4:
                    System.out.println("请输入要删除的位置");
                    i = a.input.nextInt();
                    if((e = a.delete(i))!=-1){
                        System.out.println("成功删除"+e);
                    }else{
                        System.out.println("删除失败！");
                    }
                    break;
                case 5:
                    a.print();
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

在单链表中，每个节点除了存储自身数据之外，还存储了下一个节点的地址，因此可以轻松访问下一个节点，以及后面的所有后继节点。

**但是，如果想访问前面的节点就不行了，再也回不去了。如果要向前操作，就需要用另外一种链表——双向链表**


<font color=#999AAA >下期预告：</font> 顺序队列-单向队列
