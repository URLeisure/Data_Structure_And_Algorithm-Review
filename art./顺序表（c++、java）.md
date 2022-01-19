

<font color=#999AAA >以下是本篇文章正文内容，下面案例可供参考

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


# 什么是顺序表？
>采用顺序存储的线性表称为顺序表。
>顺序表采用顺序存储方式，即逻辑上相邻的数据**在计算机内的存储位置也是相邻的**。
>根据分配空间方法不同，顺序表可以分为**静态分配**和**动态分配**两种。
>
>>**静态分配：**
>使用一个定长数组date[]存储数据，最大空间为Maxsize，用length记录实际的元素个数，即顺序表的长度。
>采用静态分配方法，定长数组需要<font color=#990033>预先分配一段固定大小的连续空间</font>，但是在运算的过程中，如合并、插入等操作，容易超过<font color=#990033>预分配</font>的空间长度，出现溢出。
>
>>**动态分配：**                                    
>在程序运行过程中，根据需要<font color=#990033>动态分配一段连续的空间</font>（大小为Maxsize），用elem记录该空间的基地址（首地址），用length记录实际的元素个数，即顺序表的长度。



下面以动态分配空间的方法为例，分别介绍顺序表的初始化、创建、取值、查找、插入、删除等基本操作。
# 顺序表的基本操作
首先定义一个结构体（内部类），包含基地址与length。

<font color=#999AAA >c++代码如下（示例）：
```cpp
struct SqList{
    int *elem;
    int length;//顺序表长度
};
```

<font color=#999AAA >java代码如下（示例）：
```java
public class Ele{
	int []elem;
	int length;//顺序表长度
}
```
## 1.初始化
为顺序表分配一段预定义大小的连续空间，用elem记录这段空间的基地址，当前空间内没有任何数据元素，因此元素的实际个数为0。

<font color=#999AAA >c++代码如下（示例）：

```cpp
bool InitList(SqList &L){//为了实现形参改变实参，使用引用
	L.elem = new int[Maxsize];
	L.length = 0;
	if(!L.elem && L.length){//添加鲁棒，判断是否成功
		return false;
	}
	return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public boolean initList(){
	s = new Ele();
	s.elem = new int[maxsize];
	if(s.elem == null)//添加鲁棒，判断是否成功
		return false;
	s.length = 0;
	return true;
}
```

## 2.创建
向顺序表中输入数据，输入数据的类型必须与类型定义中的类型一致。在创建过程中，输入-1结束。

<font color=#999AAA >c++代码如下（示例）：
```cpp
bool CreateList(SqList &L){
	int e,i = 0;
	cin >> e;
	while(e != -1){
		if(L.length == Maxsize){//鲁棒
			cout<<"顺序表已满"<<endl;
			return false;	
		}
		L.elem[i++] = e;
		L.length++;
		cin >> e;
	}
	return true;
}
```

<font color=#999AAA >java代码如下（示例）：
```java
public boolean createList(){
	int e,i = 0;
	e = input.nextInt();
	while(e != -1){
		if(s.length == maxsize){//鲁棒
			System.out.println("顺序表已满");
			return false;	
		}
		s.elem[i++] = e;
		s.length++;
		e = input.nextInt();
	}
	return true;
}
```



## 3.取值
顺序表中任何一个元素都可以立即找到，称为随机存取方式。例如，要取第i个元素，只要i值是合法的（1 ≤ i ≤ L.length），那么立即就可以找到该元素。

<font color=#999AAA >c++代码如下（示例）：
```cpp
bool GetElem(SqList L,int i,int &e){//i 为查找的第几个元素，e为此元素的值
	if(i<1 || i>L.length){
		return false;
	}
	e = L.elem[i-1];//索引从0开始，因此 -1
	return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public int getElem(int i){//i 为查找的第几个元素
	if(i<1 || i>s.length){
		return -1;
	}
	
	return s.elem[i-1];//索引从0开始，因此 -1
}
```

## 4.查找
查找一个元素e，可以从第一个元素开始顺序查找，依次比较每一个元素值。相等则返回元素位置（位序，即第几个元素）；如果没有找到，则返回-1。

<font color=#999AAA >c++代码如下（示例）：

```cpp
int LocateElem(SqList L,int e){//e 要查找的值
	for(int i = 0; i < L.length; i++){
		if(L.elem[i] == e)
			return i+1;
	}
	return -1;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public int locateElem(int e){//e 要查找的值
	for(int i = 0; i < s.length; i++){
		if(s.elem[i] == e)
			return i+1;
	}
	return -1;
}
```

## 5.插入
在顺序表第i个位置之前插入一个元素e，需要从最后一个元素开始，依次后移一位，直到把原来第i个元素也后移一位，再将e放入第i个位置。

<font color=#999AAA >代码如下（示例）：

```cpp
bool ListInsert(SqList &L,int i,int e){//i 要插入的位置，e 插入的元素值
	if(i<1 || i>L.length){
		return false;
	}
	if(L.length == Maxsize){//存储已满
		return false;
	}
	
	for(int j = L.length - 1; j >= i - 1; j--){
		L.elem[j+1] = L.elem[j];//从最后一个位置开始后移
	}
	L.elem[i-1] = e;
	L.length++;
	return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public boolean listInsert(int i,int e){//i 要插入的位置，e 插入的元素值
	if(i<1 || i>s.length){
		return false;
	}
	if(s.length == maxsize){//存储已满
		return false;
	}
	
	for(int j = s.length - 1; j >= i - 1; j--){
		s.elem[j+1] = s.elem[j];//从最后一个位置开始后移
	}
	s.elem[i-1] = e;
	s.length++;
	return true;
}
```

## 6.删除
在顺序表删除第i个元素，需要把该元素暂存到变量e中，然后从 i + 1 个元素开始依次前移，直到把最后一个元素也前移一位。

<font color=#999AAA >代码如下（示例）：

```cpp
bool ListDelete(SqList &L,int i,int &e){//e 记录要删除的元素
	if (i<1 || i>L.length) {
        return false;
    }
    e = L.elem[i-1];
    for(int j = i; j <= L.length - 1; j++){
		L.elem[j-1] = L.elem[j];
	}
	L.length--;
	return true;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public int listDelete(int i){
	int e;
	if (i<1 || i>s.length) {
        return -1;
    }
    e = s.elem[i-1];
    for(int j = i; j <= s.length - 1; j++){
		s.elem[j-1] = s.elem[j];
	}
	s.length--;
	return e;
}
```

## 7.打印
<font color=#999AAA >c++代码如下（示例）：

```cpp
void PrintList(SqList L){
	for(int i = 0; i < L.length; i++){
		i==0 || printf(" ");//利用短路原理，使末尾不含空格
		cout<<L.elem[i];
	}
	cout<<endl;
}
```

<font color=#999AAA >java代码如下（示例）：

```java
public void printList(){
	for(int i = 0; i < s.length; i++){
		System.out.print(s.elem[i]+" ");
	}
	System.out.println();
}
```

## 8.释放内存
c++需要手动释放内存，而java不需要。

<font color=#999AAA >c++代码如下（示例）：

```cpp
void DestroyList(SqList &L){
	if(L.elem){
		delete L.elem;
	}
}
```

## 9.完整代码

<font color=#999AAA >c++代码如下（示例）：

```cpp
#include<iostream>
using namespace std;
#define Maxsize 100

struct SqList{
    int *elem;
    int length;
};

bool InitList(SqList &L){
	L.elem = new int[Maxsize];
	L.length = 0;
	if(!L.elem && L.length){
		return false;
	}
	return true;
}

bool CreateList(SqList &L){
	int e,i = 0;
	cin >> e;
	while(e != -1){
		if(L.length == Maxsize){
			cout<<"顺序表已满"<<endl;
			return false;	
		}
		L.elem[i++] = e;
		L.length++;
		cin >> e;
	}
	return true;
}

bool GetElem(SqList L,int i,int &e){
	if(i<1 || i>L.length){
		return false;
	}
	e = L.elem[i-1];
	return true;
}

int LocateElem(SqList L,int e){
	for(int i = 0; i < L.length; i++){
		if(L.elem[i] == e)
			return i+1;
	}
	return -1;
}

bool ListInsert(SqList &L,int i,int e){
	if(i<1 || i>L.length){
		return false;
	}
	if(L.length == Maxsize){
		return false;
	}
	
	for(int j = L.length - 1; j >= i - 1; j--){
		L.elem[j+1] = L.elem[j];
	}
	L.elem[i-1] = e;
	L.length++;
	return true;
}

bool ListDelete(SqList &L,int i,int &e){
	if (i<1 || i>L.length) {
        return false;
    }
    e = L.elem[i-1];
    for(int j = i; j <= L.length - 1; j++){
		L.elem[j-1] = L.elem[j];
	}
	L.length--;
	return true;
}

void PrintList(SqList L){
	for(int i = 0; i < L.length; i++){
		i==0 || printf(" ");//利用短路原理，使末尾不含空格
		cout<<L.elem[i];
	}
	cout<<endl;
}

void DestroyList(SqList &L){
	if(L.elem){
		delete L.elem;
	}
}

int main(){
	SqList L;
	int choose=-1;
    int i,e,x;
	if(InitList(L)){
		cout<<"初始化成功！"<<endl;
	}else{
		cout<<"初始化失败，请检查代码！"<<endl;
		return 0;
	}
	cout << "开始创建顺序表，输入-1结束！"<<endl;
    if(CreateList(L)){
    	cout << "顺序表创建成功！"<<endl;
    }else{
		cout<<"创建失败！"<<endl;
		return 0;
	}
	cout << "1 .取值\n";
    cout << "2 .查找\n";
    cout << "3 .插入\n";
    cout << "4 .删除\n";
    cout << "5 .输出\n";
    cout << "0 .退出\n";
    while(choose != 0){
		cout<<"请选择！"<<endl;
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
                cin>>x;
                if(LocateElem(L,x)!=-1){
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
	private final int maxsize = 100;
    private Ele s;
    private static Scanner input = new Scanner(System.in);
    
	public class Ele{
		int []elem;
		int length;
	}
	
	public boolean initList(){
		s = new Ele();
		s.elem = new int[maxsize];
		if(s.elem == null)
			return false;
		s.length = 0;
		return true;
	}
	
	public boolean createList(){
		int e,i = 0;
		e = input.nextInt();
		while(e != -1){
			if(s.length == maxsize){
				System.out.println("顺序表已满");
				return false;	
			}
			s.elem[i++] = e;
			s.length++;
			e = input.nextInt();
		}
		return true;
	}

	public int getElem(int i){
		if(i<1 || i>s.length){
			return -1;
		}
		return s.elem[i-1];
	}
	
	public int locateElem(int e){
		for(int i = 0; i < s.length; i++){
			if(s.elem[i] == e)
				return i+1;
		}
		return -1;
	}
	
	public boolean listInsert(int i,int e){
		if(i<1 || i>s.length){
			return false;
		}
		if(s.length == maxsize){
			return false;
		}
	
		for(int j = s.length - 1; j >= i - 1; j--){
			s.elem[j+1] = s.elem[j];
		}
		s.elem[i-1] = e;
		s.length++;
		return true;
	}
	
	public int listDelete(int i){
		int e;
		if (i<1 || i>s.length) {
    	    return -1;
 	    }
  	  	e = s.elem[i-1];
   	 	for(int j = i; j <= s.length - 1; j++){
			s.elem[j-1] = s.elem[j];
		}
		s.length--;
		return e;
	}
	
	public void printList(){
		for(int i = 0; i < s.length; i++){
			System.out.print(s.elem[i]+" ");
		}
		System.out.println();
	}
	
    public static void main(String[] args) {
        A a = new A();
        int i,x;
        if(a.initList()){
            System.out.println("初始化成功！");
        }else{
            System.out.println("初始化失败！");
        }

        System.out.println("顺序表创建...");
        System.out.println("输入整型数，输入-1结束");
        if(a.createList()) {
            System.out.println("顺序表创建成功！");
        }else {
            System.out.println("顺序表创建失败！");
        }
        System.out.print("1. 取值\n2. 查找\n3. 插入\n4. 删除\n5. 输出\n0. 退出\n");

        int choose=-1;
        while(choose!= 0){
            System.out.println("请选择：");
            choose=input.nextInt();
            switch(choose){
                case 1:
                    System.out.println("输入整型数i，取第i个元素输出");
                    i=input.nextInt();
                    int e;
                    if((e=a.getElem(i))!=-1)
                        System.out.println("第i个元素是： "+e);
                    else
                        System.out.println("顺序表取值失败！");
                    break;
                case 2:
                    System.out.println("请输入要查找的数x:");
                    x=input.nextInt();
                    if(a.locateElem(x)==-1)
                        System.out.println("查找失败！");
                    else
                        System.out.println("查找成功！");
                    break;
                case 3:
                    System.out.println("请输入要插入的第i个位置和要插入的数据元素e:");
                    i=input.nextInt();
                    e=input.nextInt();
                    if(a.listInsert(i,e))
                        System.out.println("插入成功！");
                    else
                        System.out.println("插入失败！");
                    break;
                case 4:
                    System.out.println("请输入要删除的第i个位置:");
                    i=input.nextInt();
                    int xx;
                    if((xx = a.listDelete(i))!=-1)
                        System.out.println("成功删除"+xx);
                    else
                        System.out.println("删除失败！");
                    break;
                case 5:
                    a.printList();
                    break;
                case 0:
                    System.out.println("成功退出！");
                    break;
            }
        }
    }
}

```

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


# 顺序表的优缺点
 **优点：**
操作简单，存储密度高，可以随机存取，只需要O(1)的时间就可以取出第i个元素。

**缺点：**
需要预先分配最大空间，最大空间数估计过大或过小会造成空间浪费或溢出。插入和删除操作需要移动大量元素。


**如果经常需要插入和删除操作，则顺序表的效率很低。为了克服该缺点，可以采用链式存储**。

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">



# 总结

以上就是我的顺序表全部复习内容了，主要是为了解数组是怎么实现的。


<font color=#999AAA >下期预告：</font> 单链表
