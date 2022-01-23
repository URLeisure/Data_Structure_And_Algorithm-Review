<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## KMP 概述
KMP算法是一种改进的字符串匹配算法，由D.E.Knuth，J.H.Morris和V.R.Pratt提出的。

因此人们称它为克努特—莫里斯—普拉特操作（简称KMP算法）。

KMP算法的核心是`利用匹配失败后的信息`，尽量减少模式串与主串的匹配次数以达到快速匹配的目的。

具体实现就是通过一个next()函数实现，函数本身`包含了模式串的局部匹配信息`。

KMP算法的时间复杂度$O(m+n)$ 。
（百度百科）


## KMP 分析

在上一篇文章中（[模式匹配BF算法](https://github.com/URLeisure/Data_Structure_And_Algorithm-Review/blob/main/art./%E6%A8%A1%E5%BC%8F%E5%8C%B9%E9%85%8DBF%E7%AE%97%E6%B3%95%EF%BC%88%E5%8A%A8%E6%80%81%E5%9B%BE%E8%A7%A3%E3%80%81c%2B%2B%E3%80%81java%EF%BC%89.md))，BF 算法有点拖沓，完全没必要从 S 的每一个字符开始穷举每一种情况。

比如这一步：
                                                                         
![示例](https://img-blog.csdnimg.cn/ecd845b1418246d2ac6b255644b78f96.png)
                                                                         
按照 BF 算法，i 回退到 i - j + 1 = 4 （从零开始），j 回退到 0：
                                                                         
![BF](https://img-blog.csdnimg.cn/e052e95d9c814083a75ced71118f5286.png)
                                                                         
但通过观察，我们可以发现，** <font color=#CC3300> i 不用回退，让 j 回退到 2 号索引位即可</font> ** （就像T向右滑动了一段距离）：

![KMP](https://img-blog.csdnimg.cn/38b9149b768f4a04b59b8cfa9712380f.png)

额，有人问怎么观察出来的。

i 前面两个字符和 T 的前面两个字符是相等的。

![观察](https://img-blog.csdnimg.cn/11a92ec31b1f44208fa1fff52199d3e2.png)



## KMP next[ ]数组

### 1. 分析
**<font color=#CC3300>如何判断 T 的开头和 i 指向的前面几个字符一样？</font>**

如果两个串直接比较的话也太麻烦了，又是指数级的操作。

因为这两个串在 i、j 前已经比较过了，所以`i 前面的字符`和`j 前面的字符`是相等的，所以我们只比较 T 字符串自身就可以了。 
                                                                         
![在这里插入图片描述](https://img-blog.csdnimg.cn/a48e91b6aa9b435ca8b4bc663ca4775f.png)

假设 j 前所有字符为一个新的字符串 T`。

如果 T 的串长为 n ，我们只需要记录 n 个 T` 的公共前后缀长度即可。

这就转换成了一个新的问题，即**<font color=#CC3300>记录 n 个 T` 的最长公共前后缀</font>**。

需要注意的是：
1. **<font color=#CC3300>前缀：</font>**从前向后取若干个字符。
2. **<font color=#CC3300>后缀：</font>**从后向前取若干个字符。
3. 前缀后缀不可取字符串本身。如果串长度为 n，则前缀和后缀的长度最多为**<font color=#CC3300> n - 1</font>**。


### 2. 创建
为了更好地体现` KMP 的优势`和`next[]数组的寻找方法`，我们设 S = “abaabaabca”, T = "abaabc"。

**<font color=#CC3300>创建 next[ ] 数组，在T 和 S 进行比对时，当 S[ i ] != T[ j ] , i 不需要回退,而 j 回退到 next[ j ] ，在重新开始比较</font>。
**
next[ j ] 的含义：

1. j 的回退位置。
2.  T 串中前 j 个字符前后缀的最大匹配数。


next[ j ] 的通用公式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/ab71e0317c1543e988bb63173bef1bd4.png)

通过公式求得 next[ ] 数组。（注意：**<font color=#CC3300>T` 是 j 前面的字符串，不包含第 j 个元素</font>**）
                                                                    
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa2e0fa3411b444aa09b2b6456cb9a35.png)
                                                                    
一看就是 dp 问题。

假设，已知<font color=#CC3300>  next[ j ] = k </font>。next[ j+1 ] = 
1. **<font color=#CC3300>T~k~ = T~j~ ：</font>**next[ j+1 ] = k+1。即：下一个字符相等，那么公共长度就在上一次记录的基础上 +1。
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d59c6c22e7e4922a998c26d273bca26.png)
                                                                    
2. **<font color=#CC3300>T~k~ ≠ T~j~ ：</font>**两者不相等时，我们又开始了这两个串的模式匹配（KMP里套小KMP，太（wu）妙（yu）了）。j 不动，k 回退到 next[ k ] （公共前后缀长度）。再重新比较，直到T~k~ = T~j~ 或者回退到 k = next[ 0 ] = -1 。

### 3.代码
精妙的算法往往只需要最简单的代码来实现（我乱说的）。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Get_next(char *T,int *next){
    int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < strlen(T)-1){
        if(k==-1 || T[j]==T[k]){
            next[++j] = ++k;
        }else{
            k = next[k];//“小KMP”，模式匹配，回退到公共前后缀的下一个位置，再继续比较
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public void get_next(String t,int []next){
  	int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < t.length()-1){
        if(k==-1 || t.charAt(j)==t.charAt(k)){
            next[++j] = ++k;
        }else{
            k = next[k];//“小KMP”，模式匹配，回退到公共前后缀的下一个位置，再继续比较
        }
    }
}
```

## KMP 完整代码

主函数就不用写了吧。

特别注意：**<font color=#CC3300>在c++中，strlen() 等返回字符串长度的方法，返回值都是 'size_t' (又名 'unsigned long' )。</font>**

都是无符号整型，无符号，不能和负整型做比较！！！！

我们的 j ，在有些情况是 -1，然后就会出 bug。第一次遇到这种情况的时候，让我找了两个小时 bug，恶心死了！！！

![在这里插入图片描述](https://img-blog.csdnimg.cn/86d55834c2db4ce3bb17eb481b792b77.gif)

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Get_next(char *T,int *next){
    int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < strlen(T)-1){
        if(k==-1 || T[j]==T[k]){
            next[++j] = ++k;
        }else{
            k = next[k];
        }
    }
}

int Index_KMP(char *S,char *T,int *next){
    int i = 0,j = 0;
    int lens = strlen(S);
    int lent = strlen(T);//一定要赋给 int 不然就是无符号的
    while(i<lens && j<lent){
        if(j==-1 || S[i]==T[j]){
            i++,j++;
        }else{
            j = next[j];
        }
    }
    if(j == strlen(T)){
        return i - j;
    }
    return -1;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public void get_next(String t,int []next){
  	int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < t.length()-1){
        if(k==-1 || t.charAt(j)==t.charAt(k)){
            next[++j] = ++k;
        }else{
            k = next[k];
        }
    }
}

public int index_KMP(String s,String t,int []next) {
    int i = 0, j = 0;
   	while (i < s.length() && j < t.length()) {
        if (j == -1 || s.charAt(i) == t.charAt(j)) {
            i++;
            j++;
        } else {
            j = next[j];
       	}
    }
    if (j == t.length()) {
        return i - j;
    }
    return -1;
}
```

## KMP 优化
当 <font color=#CC3300>S[ i ] ≠ T [ j ] </font>时，j 回退到 next[ j ]。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ff618e2becf54b8da1122cd8bc6b40d4.png)

**<font color=#CC3300>但就在这时，当T[ j ] = T[ next[ j ] ] ，就是一步没有意义的回退。</font>**

因为跳转之后还要进行 S[ i ] 和 T[ j ] 的比较，T[ j ] 的值没变，肯定还是不相等，肯定还要往前跳。

![在这里插入图片描述](https://img-blog.csdnimg.cn/667f1d89accd481c893f65a46be7e61f.png)
**<font color=#CC3300>那如何减少这些无效的跳转？</font>**

太简单了，在制作 next[ ] 数组时，只要遇到相等的字符，就直接存上一次的值就行了。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void Get_next(char *T,int *next){
    int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < strlen(T)-1){
        if(k==-1 || T[j]==T[k]){
            next[++j] = ++k;
            
            if(T[j] == T[k]){//只加了这一步
                next[j] = next[k];
            }
        }else{
            k = next[k];
        }
    }
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public void get_next(String t,int []next){
  	int j = 0;
    int k = -1;
    next[0] = -1;
    while(j < t.length()-1){
        if(k==-1 || t.charAt(j)==t.charAt(k)){
            next[++j] = ++k;
            
            if(t.charAt(j) == t.charAt(k)){//只加了这一步
                next[j] = next[k];
            }
        }else{
            k = next[k];
        }
    }
}
```

## KMP 时间复杂度分析
1. 在 S 、T 比较时（ Index_KMP() 方法），i 不回退，当 S[ i ] ≠ T[ j ] 时，j 回退到 next[ j ]，重新开始比较。最坏的情况就是扫描整个S串，时间复杂度为 **<font color=#CC3300>$O(n)$</font>**。
2. 计算 next[ ] 数组时，需要扫描整个 T 串，时间复杂度为 <font color=#CC3300>$O(m)$</font>。

KMP的优化并没有降阶，因此复杂度仍为 **<font color=#CC3300>$O(n+m)$</font>**。


## BF 算法与 KMP 算法动态图解对比
同样的，S 、T 串。S = “abaabaabca”, T = "abaabc"。

1. <font color=#CC3300>BF 算法：</font>
                                         
![BF](https://img-blog.csdnimg.cn/e28eb8cc37e04c26b4d99f213cd8d921.gif)
                                         
2. <font color=#CC3300>KMP 算法：
                                         
![KMP](https://img-blog.csdnimg.cn/9ebbbd322a7040768ba0b9e52fa940b4.gif)

通过动态图我们可以看出，KMP 算法明显比 BF 算法节省很多时间。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
KMP 算法太美妙了，希望你也能搞懂。

<font color=#999AAA >下期预告：</font>特殊矩阵的存储
