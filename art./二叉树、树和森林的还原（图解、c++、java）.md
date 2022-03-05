<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


<font color=#999AAA>**根据遍历序列可以还原树，包括二叉树还原、树还原和森林还原 3 种。**</font>

## 二叉树的还原
>由二叉树的`中序序列`和`前 / 后序序列`，或者`中序序列`和`层序序列`可以唯一地还原一棵二叉树。
>
>注意：`必须包含中序序列才能唯一地还原一棵二叉树`。
>

### 前中序还原二叉树

#### 1). 算法步骤
例如：已知一棵二叉树的先序序列 $ABDECFG$ 和中序序列 $DBEAFGC$，画出二叉树。

<font color=#CC3300>**算法步骤:**</font>

1. 先序序列的第一个字符为根；
2. 在中序序列中，以根为中心划分左、右子树；
3. 还原左、右子树。

#### 2). 图解
1.找到先序的第一个字符 `A 即为根`，在中序中找到根 A 以划分左、右子树。

如图，左子树包含 DBE，右子树包含 FGC。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/e55a389675ee46a5ae5f7acdb5af2cd3.png)



2.左子树包含 DBE 对应先序序列为 BDE，所以在左子树中字符 `B 即为根`，在中序中找到根 B 以划分左、右子树。

如图，左子树包含 D，右子树包含 E。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/78aa53aee102455a90e5aae1f152ffb7.png)


3.右子树包含 FGC 对应先序序列为 CFG，所以在右子树中字符 `C 即为根`，在中序中找到根 C 以划分左、右子树。

如图，左子树包含 GF，无右子树。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c390f9764314efa8f1799809fd7e9f1.png)

4. 左子树包含 GF 对应先序序列为 FG，所以在左子树中字符 `F 即为根`，在中序中找到根 F 以划分左、右子树。

如图，无左子树，右子树包含 G。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/0a48dcd8a26d437b88409888f51bec3d.png)

完毕。

#### 3). 代码
先解释一下代码中各变量和操作，以便更好的理解代码。
1. 函数有三个参数，pre 和 mid 为字符串类型，分别指向先序、中序序列的串；len 为序列的长度。`前序和中序的序列长度一定是相同的`。
2. 先序序列的第一个字符 pre[0] 为根，然后在中序序列中查找根所在的位置，用 index 记录查找长度，找到后以根为中心，划分出左右子树。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6515077bfe704dbabe7e1efef8aae9b2.png)


`根据算法步骤可知，每次寻找左、右子树的过程都是相同的，我们可以使用递归求解即可。`

<font color=#999AAA >c++代码如下（示例）：

```cpp
void pre_mid_tree(string pre, string mid, int len, Btree &T) {
    if (len == 0) {
        T = nullptr;
        return;
    }
    char ch = pre[0];//得到根
    int index = 0;
    while (mid[index] != ch) {//查找根在中序序列中的位置
        index++;
    }
    T = new BNode;
    T->data = ch;
    pre_mid_tree(pre.substr(1), mid, index, T->lchild);
    pre_mid_tree(pre.substr(index + 1), mid.substr(index + 1), len - index - 1, T->rchild);
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static Bnode pre_mid_tree(String pre,String mid,int len){
    if(len == 0){
        return null;
    }
    char ch = pre.charAt(0);
    int index = 0;
    while(mid.charAt(index) != ch){
        index ++;
    }
    Bnode t = new Bnode();
   	t.data = ch;
    t.lchild = pre_mid_tree(pre.substring(1),mid,index);
    t.rchild = pre_mid_tree(pre.substring(index+1),mid.substring(index+1),len-index-1);
    return t;
}
```

### 中后序还原二叉树
和前中序还原蛮像的。
#### 1). 算法步骤
例如：已知一棵二叉树的中序序列 $DBEAFGC$ 和后序序列 $DEBGFCA$，画出二叉树。

<font color=#CC3300>**算法步骤:**</font>
1. 后序序列的最后一个字符为根；
2. 在中序序列中，以根为中心划分左、右子树；
3. 还原左、右子树。

#### 2). 图解
1.找到后序的最后一个字符 `A 即为根`，在中序中找到根 A 以划分左、右子树。

如图，左子树包含 DBE，右子树包含 FGC。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/546b0d09b257415592f8b39bca34bee9.png)


2.左子树包含 DBE 对应后序序列为 DEB，所以在左子树中字符 `B 即为根`，在中序中找到根 B 以划分左、右子树。

如图，左子树包含 D，右子树包含 E。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/c72ef5878b79499a80e8467aa7143dda.png)


3.右子树包含 FGC 对应后序序列为 GFC，所以在右子树中字符 `C 即为根`，在中序中找到根 C 以划分左、右子树。

如图，左子树包含 GF，无右子树。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e8cfbaef0724f33b0fff8179170af23.png)

4. 左子树包含 GF 对应后序序列为 GF，所以在左子树中字符 `F 即为根`，在中序中找到根 F 以划分左、右子树。

如图，无左子树，右子树包含 G。
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d85047ce1f646f29a687cf7f70c412d.png)


完毕。

#### 3). 代码
先解释一下代码中各变量和操作，以便更好的理解代码。
1. 函数有三个参数，pro 和 mid 为字符串类型，分别指向后序、中序序列的串；len 为序列的长度。`后序和中序的序列长度一定是相同的`。
2. 后序序列的第一个字符 pro[len-1] 为根，然后在中序序列中查找根所在的位置，用 index 记录查找长度，找到后以根为中心，划分出左右子树。

![在这里插入图片描述](https://img-blog.csdnimg.cn/307920986f8f430d9ee88bf1b85346b9.png)



`根据算法步骤可知，每次寻找左、右子树的过程都是相同的，我们可以使用递归求解即可。`

<font color=#999AAA >c++代码如下（示例）：

```cpp
void mid_pro_tree(string mid, string pro, int len, Btree &T) {
    if (len == 0) {
        T = nullptr;
        return;
    }
    char ch = pro[len - 1];//得到根
    int index = 0;
    while (mid[index] != ch) {//查找根在中序序列中的位置
        index++;
    }
    T = new BNode;
    T->data = ch;
    mid_pro_tree(mid, pro, index, T->lchild);
    mid_pro_tree(mid.substr(index + 1), pro.substr(index), len - index - 1, T->rchild);
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public static Bnode mid_pro_tree(String mid,String pro,int len){
    if(len == 0){
       	return null;
    }
    char ch = pro.charAt(len-1);
    int index = 0;
    while(mid.charAt(index) != ch){
        index ++;
    }
    Bnode t = new Bnode();
    t.data = ch;
    t.lchild = mid_pro_tree(mid,pro,index);
    t.rchild = mid_pro_tree(mid.substring(index+1),pro.substring(index),len-index-1);
    return t;
}
```

###  完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>

using namespace std;

typedef struct BNode {
    char data;
    BNode *lchild, *rchild;
} *Btree;

void pre_mid_tree(string pre, string mid, int len, Btree &T) {
    if (len == 0) {
        T = nullptr;
        return;
    }
    char ch = pre[0];
    int index = 0;
    while (mid[index] != ch) {
        index++;
    }
    T = new BNode;
    T->data = ch;
    pre_mid_tree(pre.substr(1), mid, index, T->lchild);
    pre_mid_tree(pre.substr(index + 1), mid.substr(index + 1), len - index - 1, T->rchild);
}

void mid_pro_tree(string mid, string pro, int len, Btree &T) {
    if (len == 0) {
        T = nullptr;
        return;
    }
    char ch = pro[len - 1];
    int index = 0;
    while (mid[index] != ch) {
        index++;
    }
    T = new BNode;
    T->data = ch;
    mid_pro_tree(mid, pro, index, T->lchild);
    mid_pro_tree(mid.substr(index + 1), pro.substr(index), len - index - 1, T->rchild);
}


int main() {
    Btree T;
    string pre, mid, pro;
    cout << "1.前中序还原\n2.中后序还原\n(输入序号)" << endl;
    int num;
    while (cin >> num) {
        if (num != 1 && num != 2) {
            cout << "输入错误，请重新输入！" << endl;
            continue;
        }
        break;
    }
    cout << "请输入序列：" << endl;
    if (num == 1) {
        cin >> pre >> mid;
        int len = pre.length();
        pre_mid_tree(pre, mid, len, T);
    } else {
        cin >> mid >> pro;
        int len = pro.length();
        mid_pro_tree(mid, pro, len, T);
    }
    return 0;
}
/*
前中序还原：
ABDECFG
DBEAFGC

中后序还原：
DBEAFGC
DEBGFCA
 */
```

<font color=#999AAA >java代码如下（示例）：

```java
import java.util.Scanner;

public class A {
    private static Scanner sc = new Scanner(System.in);

    public static Bnode pre_mid_tree(String pre,String mid,int len){
        if(len == 0){
            return null;
        }
        char ch = pre.charAt(0);
        int index = 0;
        while(mid.charAt(index) != ch){
            index ++;
        }
        Bnode t = new Bnode();
        t.data = ch;
        t.lchild = pre_mid_tree(pre.substring(1),mid,index);
        t.rchild = pre_mid_tree(pre.substring(index+1),mid.substring(index+1),len-index-1);
        return t;
    }

    public static Bnode mid_pro_tree(String mid,String pro,int len){
        if(len == 0){
            return null;
        }
        char ch = pro.charAt(len-1);
        int index = 0;
        while(mid.charAt(index) != ch){
            index ++;
        }
        Bnode t = new Bnode();
        t.data = ch;
        t.lchild = mid_pro_tree(mid,pro,index);
        t.rchild = mid_pro_tree(mid.substring(index+1),pro.substring(index),len-index-1);
        return t;
    }

    public static void main(String[] args) {
        String pre,mid,pro;
        Bnode t = new Bnode();
        System.out.println("1.前中序还原\n2.中后序还原\n(输入序号)");
        int num;
        do{
            num = sc.nextInt();
            if (num != 1 && num != 2) {
                System.out.println("输入错误，请重新输入！");
                continue;
            }
        }while(num != 1 && num != 2);

        System.out.println("请输入序列：");
        if (num == 1) {
            pre = sc.next();
            mid = sc.next();
            int len = pre.length();
            t = pre_mid_tree(pre, mid, len);
        } else {
            mid = sc.next();
            pro = sc.next();
            int len = pro.length();
            t = mid_pro_tree(mid, pro, len);
        }
    }

    private static class Bnode{
        char data;
        Bnode lchild,rchild;
    }
}
/*
前中序还原：
ABDECFG
DBEAFGC

中后序还原：
DBEAFGC
DEBGFCA
 */
```

## 树的还原
由于树的`先根遍历和后根遍历与其对应的二叉树的先序遍历和中序遍历相同`，因此可以根据该对应关系，`先还原为二叉树，然后再把二叉树转换成树`。
### 1). 算法步骤
<font color=#CC3300>**算法步骤:**</font>
1. 根据先根遍历和后根遍历，通过上述的 `pre_mid_tree()` 方法还原二叉树。
2. 将二叉树转换成树。
### 2). 图解

没图解了，二叉树树转换成树不会的可以看这篇 -> [树的存储（图解、树、森林与二叉树的转换）](https://blog.csdn.net/weixin_50564032/article/details/122521092)

## 森林的还原
由于森林的`先序遍历和中序遍历与其对应的二叉树的先序遍历和中序遍历相同`，因此可以根据该对应关系，`先还原为二叉树，然后再把二叉树转换成森林`。
### 1). 算法步骤
<font color=#CC3300>**算法步骤:**</font>
1. 根据先序遍历和中序遍历，通过上述的 `pre_mid_tree()` 方法还原二叉树。
2. 将二叉树转换成森林。
### 2). 图解
也没图解了，二叉树树转换成树不会的可以看这篇 -> [树的存储（图解、树、森林与二叉树的转换）](https://blog.csdn.net/weixin_50564032/article/details/122521092)
## 总结
要还原唯一二叉树`必须要有中序遍历`。
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<font color=#999AAA >下期预告：</font>哈夫曼树
