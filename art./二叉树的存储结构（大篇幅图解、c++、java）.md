
<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 二叉树的存储
###  顺序存储
二叉树也可以采用顺序存储，按照完全二叉树的节点层次编号，依次存放二叉树中的数据元素。

<font color=#CC3300>**完全二叉树</font>很适合顺序存储方式，下图所示的完全二叉树的顺序存储结构如图所示。**
                                                                         
![完全二叉树顺序存储](https://img-blog.csdnimg.cn/93a38ad332054b35b53bcdd16aa32f43.png)
                                                                         
而<font color=#CC3300>**普通二叉树</font>在顺序存储时需要补充为完全二叉树，在对应完全二叉树没有孩子的位置补0，如下图。**
                                                                         
![普通二叉树顺序存储](https://img-blog.csdnimg.cn/5331655c5f0c42f4ad575796b7320f92.png)
                                                                         
显然，普通二叉树不适合顺序存储方式，因为有可能在补充为完全二叉树过程中，补充太多的0，而浪费大量空间，因此不同二叉树一般使用链式存储。
                                                                         
### 链式存储
                                                                         
二叉树最多有两个“叉”，即最多有两棵子树。

二叉树采用链式存储方式：

<font color=#CC3300>**每个节点包含一个数据域，存储节点信息；还包含两个指针域，指向左右两个孩子。**</font>
                                                                         
![一个节点](https://img-blog.csdnimg.cn/e1aa284216a34ac4ba136e09f9976c11.png)

于是，普通的二叉树就可以存储为二叉链表的形式，如图。
                                                                         
![链表的](https://img-blog.csdnimg.cn/e13fa24c5d7e4b918cfd48ea1471623c.png)

一般情况下，二叉树采用二叉链表存储即可，但是在实际问题中，如果经常需要访问双亲节点，二叉链表存储则必须从根出发查找其双亲节点，这样做非常麻烦。

这时可以采用，三叉链表。
                                                                         
![三叉链表](https://img-blog.csdnimg.cn/bf3e9ff0999b4d0498779619b6963e7c.png)
                                                                         
再画个图，好多箭头，画起来好麻烦啊！！
                                                                         
![三叉链表树](https://img-blog.csdnimg.cn/26bf6d35340046b9aff99df0135c0d5d.png)

## 二叉树的创建
从二叉树的定义可以看出，它是递归定义的（除了根之外，左、右子树也是一棵二叉树），因此可以用递归来创建二叉树。

递归创建二叉树有两种方法，分别是<font color=#CC3300>`询问法`</font>和<font color=#CC3300>`补空法`</font>。

询问法是为了更好的理解二叉树创建的过程，要是直接复制代码去做题，hh。
                                                                         
### 询问法
                                                                         
1. 每次输入节点信息后，询问是否创建该节点的左子树。
如果是，则递归创建其左子树，否则左子树为空。

2. 询问是否创建该节点的右子树。
如果是，则递归创建其右子树，否则其右子为空

<font color=#CC3300>**`算法步骤`**</font>
1. 输入节点信息，创建一个节点T。
2. 询问是否创建T的左子树，如果是，则递归创建其左子树，否则其左子树为NULL。
3. 询问是否创建T的右子树，如果是，则递归创建其右子树，否则其右子树为NULL。


**例如，输入下图二叉树。**
                                                                         
![二叉树样例](https://img-blog.csdnimg.cn/32a9abf0abd4485d92f02a8d2757927b.png)
                                                                         
输入为：(java中要换行输入)
$$
AYBYDNNYENNYCYFNYGNNN
$$
#### 1). 图解
1. 请输入节点信息：A
输入后创建节点A，如图。（谁强迫症，来领死。我就弄一根长一根短）
                                                                         
![A](https://img-blog.csdnimg.cn/fe563bfd7c9f4ffd9da206235191567b.png) 
                                                                         
2. 是否添加A的左孩子?(Y/N) Y
3. 请输入节点信息：B
输入后创建节点B，作为A的左孩子，如图。(这图难受吗？)

![B](https://img-blog.csdnimg.cn/a17b2428b6eb4c15b926c72056ba53f8.png)

4. 是否添加B的左孩子？(Y/N) Y
5. 请输入节点信息：D
输入后创建节点D，作为B的左孩子，如图。
                                                                         
![D](https://img-blog.csdnimg.cn/204217504dc64e25a0a388d3be7e3baa.png)
                                                                         
6. 是否添加D的左孩子？(Y/N) N
7. 是否添加D的右孩子？(Y/N) N
输入后D的左右孩子均为空，如图。
                                                                         
![D](https://img-blog.csdnimg.cn/7811f8ebb94442178f0ca63433d95935.png)
                                                                         
8. 是否添加B的右孩子？(Y/N) Y
9. 请输入节点信息：E
 输入后创建节点E，作为B的右孩子，如图。
                                                                         
![E](https://img-blog.csdnimg.cn/bfa116632c9d404491643a342b4129e1.png)
                                                                         
10. 是否添加E的左孩子？(Y/N) N
11. 是否添加E的右孩子？(Y/N) N
输入后E的左右孩子均为空，如图。
                                                                         
![E](https://img-blog.csdnimg.cn/fbb779d25246427d97ea4e120f7491c3.png)
                                                                         
12. 是否添加A的右孩子？(Y/N) Y
13. 请输入节点信息：C
 输入后创建节点C，作为A的右孩子，如图。
                                                                         
 ![C](https://img-blog.csdnimg.cn/d1fe221f7bb54030a9b836bf58ba7950.png)
                                                                         
14. 是否添加C的左孩子？(Y/N) Y
15. 请输入节点信息：F
 输入后创建节点F，作为C的左孩子，如图。
                                                                         
 ![F](https://img-blog.csdnimg.cn/fd68739d67854214a9160909609a3071.png)
                                                                         
16. 是否添加F的左孩子？(Y/N) N
即F的左孩子为空，如图。
                                                                         
![F](https://img-blog.csdnimg.cn/7911fd90884a4909bae0e8a29dcfb2a9.png)

17. 是否添加F的右孩子？(Y/N) Y
18.  请输入节点信息：G
 输入后创建节点G，作为F的左孩子，如图。
                                                                         
![G](https://img-blog.csdnimg.cn/9fb4eaf20e8348c19ba2e10c65b3818a.png)
                                                                         
19. 是否添加G的左孩子？(Y/N) N
20. 是否添加G的右孩子？(Y/N) N
输入后G的左右孩子均为空，如图。

![G](https://img-blog.csdnimg.cn/32b679b386fa4c94b8470c85a9a999b5.png)

21. 是否添加C的右孩子？(Y/N) N
 输入后C的右孩子均为空，如图。
                                                                         
![C](https://img-blog.csdnimg.cn/a116b2f3ebee41dea5c93be2d1395ecc.png)
                                                                         
22. 二叉树创建完毕。（真累）

#### 2). 代码
图很多，来个递归就几行= =。

先创建一个结构体（内部类），包含数据域，和两个孩子指针域。

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct BNode{
    char data;
    BNode *lchild,*rchild;
}*Btree;
```
<font color=#999AAA >java代码如下（示例）：

```java
public static class BNode{
    String data;
    BNode lchild,rchild;
}
```


<font color=#CC3300>**创建树**</font>

<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateTree(Btree &T){
    char ch;
    cout<<"请输入节点信息："<<endl;
    T = new BNode;
    cin>>T->data;

    cout<<"是否添加【"<<T->data<<"】的左孩子?(Y/N)"<<endl;
    cin>>ch;
    if(ch == 'Y'){
        CreateTree(T->lchild);
    }else{
        T->lchild = NULL;
    }

    cout<<"是否添加【"<<T->data<<"】的右孩子?(Y/N)"<<endl;
    cin>>ch;
    if(ch == 'Y'){
        CreateTree(T->rchild);
    }else{
        T->rchild = NULL;
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public BNode createTree(BNode T){
    String ch;
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入节点信息：");
    T = new BNode();
    T.data= sc.nextLine();//用Scanner.nextLine()的时候一定要注意，换行输入
    System.out.println("是否添加【"+T.data+"】的左孩子?(Y/N)");
    ch = sc.nextLine();
    if(ch.equals("Y")){
        T.lchild = createTree(T.lchild);//跟c++的递归不太一样，注意java的递归“陷阱”
    }else{
        T.lchild = null;
    }

    System.out.println("是否添加【"+T.data+"】的右孩子?(Y/N)");
    ch = sc.nextLine();
    if(ch.equals("Y")){
        T.rchild = createTree(T.rchild);//跟c++的递归不太一样，注意java的递归“陷阱”
    }else{
        T.rchild = null;
    }
    return T;
}
```
### 补空法
补空法时值如果左子树或右子树为空时，则用特殊字符补空，如“#”，然后按照根、左子树、右子树的顺序，得到<font color=#CC3300>`先序遍历`</font>序列，根据该序列递归创建二叉树。

<font color=#CC3300>**算法步骤**</font>

1. 输入补空后的二叉树先序遍历序列。
2. 如果输入ch == “#”，T = NULL；否则创建一个新节点T，令T.data = ch；递归创建T的左子树；递归创建T的右子树。


**例如，输入下图二叉树。**
                    
![二叉树样例](https://img-blog.csdnimg.cn/32a9abf0abd4485d92f02a8d2757927b.png)
                    
输入为：(java中要换行输入)
$$
ABD\#\#E\#\#CF\#G\#\#\#
$$

#### 1). 图解
又要画好多图了。

1. 读取第1个字符A，创建一个新节点，如图。然后递归创建A的左子树。

![A](https://img-blog.csdnimg.cn/fe563bfd7c9f4ffd9da206235191567b.png)
 
2. 读取第2个字符B，创建一个新节点，作为A的左子树，如图。然后递归创建B的左子树。

![B](https://img-blog.csdnimg.cn/a17b2428b6eb4c15b926c72056ba53f8.png)

3. 读取第3个字符D，创建一个新节点，作为B的左子树，如图。然后递归创建B的左子树。

![D](https://img-blog.csdnimg.cn/204217504dc64e25a0a388d3be7e3baa.png)

4. 读取第4个字符#，说明D的左子树为空。然后递归创建D的右子树。
5. 读取第5个字符#，说明D的右子树为空，如图。然后递归创建B的右子树。
                    
![D](https://img-blog.csdnimg.cn/7811f8ebb94442178f0ca63433d95935.png)
                    
6. 读取第6个字符E，创建一个新节点，作为B的右子树，如图。然后递归创建E的左子树。
                    
![E](https://img-blog.csdnimg.cn/bfa116632c9d404491643a342b4129e1.png)

7. 读取第7个字符#，说明E的左子树为空。然后递归创建E的右子树。
8. 读取第8个字符#，说明E的右子树为空，如图。然后递归创建A的右子树。
                    
![E](https://img-blog.csdnimg.cn/fbb779d25246427d97ea4e120f7491c3.png)
                    
9. 读取第9个字符C，创建一个新节点，作为A的右子树，如图。然后递归创建C的左子树。


![C](https://img-blog.csdnimg.cn/d1fe221f7bb54030a9b836bf58ba7950.png)
 
10. 读取第10个字符F，创建一个新节点，作为C的左子树，如图。然后递归创建F的左子树。
                    
 ![F](https://img-blog.csdnimg.cn/fd68739d67854214a9160909609a3071.png)
                    
11.  读取第11个字符#，说明F的左子树为空。如图，然后递归创建F的右子树。

![F](https://img-blog.csdnimg.cn/7911fd90884a4909bae0e8a29dcfb2a9.png)

12. 读取第12个字符G，创建一个新节点，作为F的右子树，如图。然后递归创建G的左子树。

![G](https://img-blog.csdnimg.cn/9fb4eaf20e8348c19ba2e10c65b3818a.png)

13. 读取第13个字符#，说明G的左子树为空。然后递归创建G的右子树。
14. 读取第14个字符#，说明G的右子树为空，如图。然后递归创建C的右子树。

![G](https://img-blog.csdnimg.cn/32b679b386fa4c94b8470c85a9a999b5.png)

15.  读取第15个字符#，说明C的右子树为空，如图。
                    
![C](https://img-blog.csdnimg.cn/a116b2f3ebee41dea5c93be2d1395ecc.png)

序列读取完毕，二叉树创建成功。（真累，假的）

#### 2). 代码

先创建一个结构体（内部类），包含数据域，和两个孩子指针域。(不能说跟上面很像，只能说跟上面一模一样)

<font color=#999AAA >c++代码如下（示例）：

```cpp
typedef struct BNode{
    char data;
    BNode *lchild,*rchild;
}*Btree;
```
<font color=#999AAA >java代码如下（示例）：

```java
public static class BNode{
    String data;
    BNode lchild,rchild;
}
```


<font color=#CC3300>**创建树**</font>

<font color=#999AAA >c++代码如下（示例）：

```cpp
void CreateTree(Btree &T){
    char ch;
    cin>>ch;
    if(ch != '#'){
        T = new BNode;
        T->data = ch;
        CreateTree(T->lchild);
        CreateTree(T->rchild);
    }else{
        T = NULL;
    }
}
```
<font color=#999AAA >java代码如下（示例）：

```java
public BNode createTree(BNode T){
    Scanner sc = new Scanner(System.in);
    String ch = sc.nextLine();
    if(ch.equals("#")){
        T = null;
    }else{
        T = new BNode();
        T.data = ch;
        T.lchild = createTree(T.lchild);
        T.rchild = createTree(T.rchild);
    }
    return T;
}
```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
之后会有递归的文章，好几天之后。

<font color=#CC3300>画图不容易，点个免费的赞吧。</font>

<font color=#999AAA >下期预告：</font>二叉树的遍历
