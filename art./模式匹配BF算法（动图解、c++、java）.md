<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考。</fond>
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



## 什么是模式匹配？
>字串的定位运算称为<font color=#CC3300>**串的模式匹配**</font> 或<font color=#CC3300>**串匹配**</font> 。
>
>假设有两个串 S、T，设 S 为主串，也称正文串；T 为子串，也称模式。
>
>在主串S中查找与模式T相匹配的子串，如果查找成功，返回匹配的子串第一个字符在主串中的位置。
>
>最笨的办法就是<font color=#CC3300>**穷举所有S的所有子串，判断是否与T匹配**，该算法称为**BF（Brute Force）算法**。</font> 


## 模式匹配 BF 算法步骤（动图）
1. 从 S 第0个字符开始，与T第0个字符比较。
如果相等，继续比较下一个字符，否则跳转向下一步；
2. 从 S 第1个字符开始，与T第0个字符比较。
如果相等，继续比较下一个字符，否则跳转向下一步；
3. 从 S 第2个字符开始，与T第0个字符比较。
如果相等，继续比较下一个字符，否则跳转向下一步；
...

</font>

1. 如果 T 比较完毕，则返回 T 在 S 中第一个字符出现的索引（从零开始）；
2. 如果 S 比较完毕，则返回 -1，说明 T 在 S 中未出现。

**设，S = “abcabdcababdcabeac”，T = “abdcabe”，求子串 T 在主串 S 中位置。**

先来一遍文字说明，再上图解。

1. S[0] == T[0] , S[1] == T[1] , S[2] != T[2] , 跳转下一步；
2. S[1] != T[0] , 跳转下一步；
3. S[2] != T[0] , 跳转下一步；
4. S[3] == T[0] , S[4] == T[1] , S[5] == T[2] , S[6] == T[3] , S[7]  == T[4] , S[8] == T[5] , S[9] != T[6] 跳转下一步；
5. S[4] != T[0] , 跳转下一步；
6. S[5] != T[0] , 跳转下一步；
7. S[6] != T[0] , 跳转下一步；
8. S[7] == T[0] , S[8] == T[1] , S[9] != T[2] , 跳转下一步；
9. S[8] != T[0] , 跳转下一步；
10. S[9] == T[0] , S[10] == T[1] , S[11] == T[2] , S[12] == T[3] , S[13] == T[4] , S[14] == T[5] , S[15] == T[6] ；
11. T 比较完毕，返回 9。
                                                                         

![BF](https://img-blog.csdnimg.cn/5ac67c3d9a1e420f995ea37f54d0c231.gif)


## 简单的匹配代码
从算法步骤部分不难看出：

1. i 的回退位置为 <font color=#CC3300> **i - j + 1** </font> 。
2. j 的回退位置为 <font color=#CC3300> **0** </font> 。


<font color=#999AAA >c++代码如下（示例）：

```cpp
int Index_BF(char *S,char *T){
    int i = 0,j = 0;
    while(i < strlen(S) && j < strlen(T)){
        if(S[i] == T[j]){
            i++,j++;
        }else{
            i = i - j + 1;
            j = 0;
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
public int index_BF(String s,String t){
    int i = 0,j = 0;
   	while(i<s.length() && j<t.length()){
        if(s.charAt(i) == t.charAt(j)){
            i++;
            j++;
        }else{
            i = i - j + 1;
            j = 0;
        }
    }

    if(j == t.length()){
        return i - j;
    }
    return -1;
}
```

## BF 算法复杂度分析
1.<font color=#CC3300> **最好情况：** </font>
	
例如：S = "abcdefg" ，T = “def”。

此时，在匹配时，i 、j 都不需要回退。

平均时间复杂度为 <font color=#CC3300> **O(n + m)** </font>。

2.<font color=#CC3300> **最坏情况：** </font>  

例如：S = "aaaab" ，T = “ab”。

此时，在最后一次匹配前，i、j 一直在回退。

平均时间复杂度为 <font color=#CC3300> **O(n × m)** </font>。
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## 总结
在示例比较时，有一步比较为：

![示例](https://img-blog.csdnimg.cn/ecd845b1418246d2ac6b255644b78f96.png)

通过模式匹配BF算法，i、j 回退为：

![BF](https://img-blog.csdnimg.cn/e052e95d9c814083a75ced71118f5286.png)

但通过观察，我们可以发现，我们完全可以直接这么比较：

![KMP](https://img-blog.csdnimg.cn/38b9149b768f4a04b59b8cfa9712380f.png)

来达到，i 不回退，只 j 回退的目的，大大减少了时间复杂度。

此方法便是 <font color=#CC3300> **KMP算法** </font>  。

虽然没人看，但还是想说一下：

本来打算明天更新的，但是有些事情需要处理一下，明天更不了 KMP 了 QAQ。

<font color=#999AAA >下期预告：</font>模式匹配KMP算法
