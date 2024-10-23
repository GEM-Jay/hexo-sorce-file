---
title: 用JAVA实现银行家算法
tags:
  - Java
  - 算法
categories:
  - 算法
repo: GEM-Jay/GEM-Jay.github.io
abbrlink: 56575920
date: 2024-10-16 12:49:56
---
# 用Java实现银行家算法

## 实验目的

银行家算法是避免死锁的一种重要方法，本实验要求用高级语言编写和调试一个简单的银行家算法程序。加深了解有关资源申请、避免死锁等概念，并体会和了解死锁和避免死锁的具体实施方法。

## 实验内容

1\)设计进程对各类资源最大申请表示及初值确定。  
2\)设定系统提供资源初始状况。  
3\)设定每次某个进程对各类资源的申请表示。  
4\)编制程序，依据银行家算法，决定其申请是否得到满足。

## 代码解释

### Banker类-T0时刻资源分配

进行变量定义：  
对进程数量M，资源种类数量N，各资源总数All\[\]，m个进程对n类资源的最大需求量Max\[\]\[\]，m个进程已经得到n类资源的资源量Allocation\[\]\[\],m个进程还需要n类资源的资源量Need\[\]\[\],系统可用资源数系统可用资源数Available\[\]\[\]。还有一个进程完成标志位Finish\[\]。

```none
class Banker{

	int ID[],			//进程代号
		M,				//m个进程
		N,				//n类资源
		All[],			//系统各资源数量
		Max[][],		//m个进程对n类资源的最大需求量
		Allocation[][],	//m个进程已经得到n类资源的资源量
		Need[][],		//m个进程还需要n类资源的资源量
		Available[][];	//系统可用资源数
		
	boolean Finish[];	//标记进程是否完成
	int a=0;//Available的第一个下标
```

无参构造函数：  
在无参函数中进行变量的初始化，需要手动输入的变量为M,N，All\[\]，Max\[\]\[\]，Allocation\[\]\[\]。  
当输入了初始值后，Need\[\]\[\]和Available\[\]\[\]就可以通过need 的计算公式：  
Need=\[i\]\[j\]= Max\[i\]\[j\]- Allocation\[i\]\[j\]  
Available\[\]\[\]：Available\[a\]\[n\] = All\[n\] \- Allocation\[m\]\[n\]（遍历）

```none
public Banker() {	//无参构造器
		@SuppressWarnings("resource")
		Scanner input = new Scanner(System.in);
        System.out.println("请输入进程数");
		M = input.nextInt();//m个进程
		System.out.println("请输入资源种类数");
		N = input.nextInt();//n类资源
		//初始化数组
		ID = new int[M];			//进程代号
		All = new int[N];			//系统各资源数量
		Max = new int[M][N];		//m个进程对n类资源的最大需求量
		Allocation = new int[M][N];	//m个进程已经得到n类资源的资源量
		Need = new int[M][N];		//m个进程还需要n类资源的资源量
		Available = new int[M+1][N];		//系统可用资源数
		Finish = new boolean[M];	//标记进程是否完成
		System.out.println("请输入系统初始可用资源数");
		for(int i=0; i<N; i++)//系统初始可用资源数
			All[i] = input.nextInt();
		System.out.println("请输入"+M+"个进程对"+N+"类资源的最大需求量");
		for(int i=0; i<M; i++){//m个进程对n类资源的最大需求量
			ID[i] = i;
			for(int j=0; j<N; j++)
				Max[i][j] = input.nextInt();
		}
		System.out.println("请输入"+M+"个进程已经得到的"+N+"类资源的资源量");
		for(int i=0; i<M; i++)	//m个进程已经得到n类资源的资源量
			for(int j=0; j<N; j++)
				Allocation[i][j] = input.nextInt();
		Need_Resources();
		Available_Resources();
		Print_Banker();
	}
```

初始化Need和Available矩阵：

```none
private void Need_Resources() {//初始化Need矩阵
		// TODO Auto-generated method stub
		for(int i=0; i<M; i++)//m个进程还需要n类资源的资源量
			for(int j=0; j<N; j++)
				Need[i][j] = Max[i][j] - Allocation[i][j];
	}

	private void Available_Resources() {//更新系统当前可用资源数
		// TODO Auto-generated method stub
		for(int n=0; n<N; n++){//系统目前可用资源数
			Available[a][n] = All[n];
			for(int m=0; m<M; m++){
				Available[a][n] -= Allocation[m][n];
			}
		}
	}
```

就此，所有矩阵初始化完毕，对T0时刻的资源分配图进行打印，如图所示：  
![运行截图](https://cdn.jsdelivr.net/gh/GEM-Jay/images/T0%E7%9F%A9%E9%98%B5.png)

```none
private void Print_Banker() {//T0时刻的资源分配图
		System.out.println("\nT0时刻资源分配图：");
		System.out.print("资源\t资源数量\n");
		for(int i=0; i<N; i++)
			System.out.printf("S%d\t%d\n", i, All[i]);
		System.out.print("\n进程\t Max\tAllocation\tNeed\tAvailable\tFinish\n");
		for(int i=0; i<M; i++){
			System.out.print("P"+ID[i]);
			System.out.print("\t");
			for(int j=0; j<Max[ID[i]].length; j++){
				System.out.printf("%d ", Max[ID[i]][j]);
			}
			System.out.print("\t");
			for(int j=0; j<Allocation[ID[i]].length; j++){
				System.out.printf("%d ", Allocation[ID[i]][j]);
			}
			System.out.print("\t\t");
			for(int j=0; j<Need[ID[i]].length; j++){
				System.out.printf("%d ", Need[ID[i]][j]);
			}
			System.out.print("\t");
			if(i == 0){
				for(int j=0; j<N; j++){
					System.out.printf("%d ", Available[i][j]);
				}
			}
			System.out.print("\t");
			System.out.print("\t");
			System.out.print(Finish[i]);
			System.out.println();
		}
	}
```

#### 安全性检查

对T0时刻资源分配情况进行安全性分析，为每个进程进行可满足性检查。  
Need\[i\]\[j\] > Available\[a\]\[j\]  
遍历每个资源，都满足可满足性，则进入试分配，若不满足，将该进程的标志位flag2置为false，跳出for循环。试分配，先将进程的Finish标志位置为true，代表这个进程已经被分配资源执行完毕。  
更新Available：  
Available\[a\]\[j\] = Available\[a-1\]\[j\] + Allocation\[i\]\[j\];  
当所有进程都通过试分配\(a == M\)后，打印分配过资源的安全序列Print\_Banker\_Se\(\)

```none
public void Security_examine(){//安全性检测
		boolean flag1,	//所有进程
				flag2;	//每个进程
		flag1 = true;
		while(flag1){
			flag1 = false;
			for(int i=0; i<M; i++){
				flag2 = true;
				for(int j=0; flag2 && j<N; j++){
					if(Need[i][j] > Available[a][j] || Finish[i]){//存在一个条件不满足或者该进程已经完成
						flag2 = false;
					}
				}
				if(flag2 && !Finish[i]){//该进程（第i个进程）可执行
					flag1 = true;
					Finish[i] = true;
					ID[a] = i;
					a++;//以此判断所有进程是否都执行，安全检查
					for(int j=0; flag2 && j<N; j++){
						Available[a][j] = Available[a-1][j] + Allocation[i][j];
					}
				}
			}
		}
		System.out.println("\nT0时刻的安全性：");
		if(a == M){
			Print_Banker_Se();
			System.out.println("\n安全序列：");
			for(int i=0; i<M; i++){
				System.out.print("->P"+ID[i]);
			}
			System.out.println("\n");
		}
		else{
			System.out.println("系统不安全");
		}
	}
```

* * *

### Banker类-银行家算法

申请资源：某个进程发起请求向量Request\(_,_,\*\)，系统按银行家算法进行检查。  
① Request\[i\] > Need\[n\]\[i\]  
② Request\[i\] > Available\[0\]\[i\]  
满足以上两条说明不满足正确性检查和可满足性检查，将标志位flag置为false，报告分配不安全。  
若通过了正确性和可满足性检测就将这条进程的已分配Allocation加上申请量，并重新对Need和Available矩阵进行新的初始化。并打印此时初状态资源分配表。

安全性检查：  
通过主程序调用键盘输入控制进入安全性检查部分，思想和源码与T0时刻的安全性检查雷同，再次不在赘述。[安全性检查](#%E5%AE%89%E5%85%A8%E6%80%A7%E6%A3%80%E6%9F%A5)

```none
public void Reallocation(){//申请资源
		@SuppressWarnings("resource")
		Scanner input = new Scanner(System.in);
		int[] Request = new int[N];
		boolean flag = true;
		System.out.print("\n输入进程代号：");
		int n = input.nextInt();
		System.out.print("输入请求资源数：");
		for(int j=0; j<N; j++)
			Request[j] = input.nextInt();
		for(int i=0; i<N; i++){//合理性检查,可用性检查
			if(Request[i] > Need[n][i] || Request[i] > Available[0][i])
				flag = false;
		}
		if(flag){
			for(int i=0; i<N; i++){
				Allocation[n][i] += Request[i];
			}
			Init();
			Print_Banker();
		}
		else{
			System.out.println("分配不安全\n");
		}	
	}

	private void Init() {
		// TODO Auto-generated method stub
		a = 0;
		Need_Resources();//再次初始化
		Available_Resources();
		for(int i=0; i<M; i++){//ID初始化
			ID[i] = i;
			Finish[i] = false;
		}
	}
```

### 主函数

使用无参构造器new了一个banker对象，打印菜单，并控制调用安全性检测、银行家算法（请求再分配资源）以及退出。

```none
public class TestBankerClass {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Banker banker = new Banker();
		int n;
		boolean flag=true;
		Scanner input = new Scanner(System.in);
		System.out.println("\n*************菜单****************");
		System.out.println("进行安全性检查：1\n请求再分配资源：2\n退出：其他");
		System.out.println("*********************************");
		while(flag){
			System.out.print("输入操作代号：");
			n = input.nextInt();
			switch(n){
				case 1:	banker.Security_examine();break;
				case 2:	banker.Reallocation();break;
				default:flag = false;
				System.out.print("\n操作结束！！！！");
			}
		}
		input.close();//关闭流
	}
}
```

## 输入示例

例1：

```none
初始化：
5
3
10 5 7

7 5 3
3 2 2
9 0 2
2 2 2
4 3 3

0 1 0
2 0 0
3 0 2
2 1 1
0 0 2

再分配：
1
1 0 2

4
3 3 0

0
0 2 0
```

例2：

```none
初始化：
3
3
10 8 7
8 7 5
5 2 5
6 6 2

3 2 0
2 0 2
1 3 2

再分配：
0
1 0 0

1
0 1 1
```

## 运行结果

例1：

```none
T0时刻资源分配图：
资源	资源数量
S0	10
S1	5
S2	7

进程	 Max	Allocation	Need	Available	Finish
P0	7 5 3 	0 1 0 		7 4 3 	3 3 2 		false
P1	3 2 2 	2 0 0 		1 2 2 			false
P2	9 0 2 	3 0 2 		6 0 0 			false
P3	2 2 2 	2 1 1 		0 1 1 			false
P4	4 3 3 	0 0 2 		4 3 1 			false

*************菜单****************
进行安全性检查：1
请求再分配资源：2
退出：其他
*********************************
输入操作代号：1

T0时刻的安全性：

进程	 Work	Need	Allocation	Work+Allocation	Finish
P1	3 3 2 	1 2 2 	2 0 0 		5 3 2 		true
P3	5 3 2 	0 1 1 	2 1 1 		7 4 3 		true
P4	7 4 3 	4 3 1 	0 0 2 		7 4 5 		true
P0	7 4 5 	7 4 3 	0 1 0 		7 5 5 		true
P2	7 5 5 	6 0 0 	3 0 2 		10 5 7 		true

安全序列：
->P1->P3->P4->P0->P2

输入操作代号：2

输入进程代号：1
输入请求资源数：1 0 2

T0时刻资源分配图：
资源	资源数量
S0	10
S1	5
S2	7

进程	 Max	Allocation	Need	Available	Finish
P0	7 5 3 	0 1 0 		7 4 3 	2 3 0 		false
P1	3 2 2 	3 0 2 		0 2 0 			false
P2	9 0 2 	3 0 2 		6 0 0 			false
P3	2 2 2 	2 1 1 		0 1 1 			false
P4	4 3 3 	0 0 2 		4 3 1 			false
输入操作代号：1

T0时刻的安全性：

进程	 Work	Need	Allocation	Work+Allocation	Finish
P1	2 3 0 	0 2 0 	3 0 2 		5 3 2 		true
P3	5 3 2 	0 1 1 	2 1 1 		7 4 3 		true
P4	7 4 3 	4 3 1 	0 0 2 		7 4 5 		true
P0	7 4 5 	7 4 3 	0 1 0 		7 5 5 		true
P2	7 5 5 	6 0 0 	3 0 2 		10 5 7 		true

安全序列：
->P1->P3->P4->P0->P2

输入操作代号：2

输入进程代号：4
输入请求资源数：3 3 0
分配不安全

输入操作代号：1

T0时刻的安全性：

进程	 Work	Need	Allocation	Work+Allocation	Finish
P1	2 3 0 	0 2 0 	3 0 2 		5 3 2 		true
P3	5 3 2 	0 1 1 	2 1 1 		7 4 3 		true
P4	7 4 3 	4 3 1 	0 0 2 		7 4 5 		true
P0	7 4 5 	7 4 3 	0 1 0 		7 5 5 		true
P2	7 5 5 	6 0 0 	3 0 2 		10 5 7 		true

安全序列：
->P1->P3->P4->P0->P2

输入操作代号：2

输入进程代号：0
输入请求资源数：0 2 0

T0时刻资源分配图：
资源	资源数量
S0	10
S1	5
S2	7

进程	 Max	Allocation	Need	Available	Finish
P0	7 5 3 	0 3 0 		7 2 3 	2 1 0 		false
P1	3 2 2 	3 0 2 		0 2 0 			false
P2	9 0 2 	3 0 2 		6 0 0 			false
P3	2 2 2 	2 1 1 		0 1 1 			false
P4	4 3 3 	0 0 2 		4 3 1 			false
输入操作代号：1

T0时刻的安全性：
系统不安全
输入操作代号：4

操作结束！！！！
```

## 完整代码

TestBankerClass类：

```none
package bankerTest;

import java.util.Scanner;

public class TestBankerClass {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Banker banker = new Banker();
		int n;
		boolean flag=true;
		Scanner input = new Scanner(System.in);
		System.out.println("\n*************菜单****************");
		System.out.println("进行安全性检查：1\n请求再分配资源：2\n退出：其他");
		System.out.println("*********************************");
		while(flag){
			System.out.print("输入操作代号：");
			n = input.nextInt();
			switch(n){
				case 1:	banker.Security_examine();break;
				case 2:	banker.Reallocation();break;
				default:flag = false;
				System.out.print("\n操作结束！！！！");
			}
		}
		input.close();//关闭流
	}
}
```

Banker类：

```none
package bankerTest;

import java.util.Scanner;

class Banker{
	//定义数组
	int ID[],			//进程代号
		M,				//m个进程
		N,				//n类资源
		All[],			//系统各资源数量
		Max[][],		//m个进程对n类资源的最大需求量
		Allocation[][],	//m个进程已经得到n类资源的资源量
		Need[][],		//m个进程还需要n类资源的资源量
		Available[][];	//系统可用资源数
	boolean Finish[];	//标记进程是否完成
	int a=0;			//Available的第一个下标

	public Banker() {	//无参构造器
		@SuppressWarnings("resource")//忽视输入流未关闭的报错
		Scanner input = new Scanner(System.in);
        System.out.println("请输入进程数");
		M = input.nextInt();//m个进程
		System.out.println("请输入资源种类数");
		N = input.nextInt();//n类资源
		//初始化数组
		ID = new int[M];			//进程代号
		All = new int[N];			//系统各资源数量
		Max = new int[M][N];		//m个进程对n类资源的最大需求量
		Allocation = new int[M][N];	//m个进程已经得到n类资源的资源量
		Need = new int[M][N];		//m个进程还需要n类资源的资源量
		Available = new int[M+1][N];		//系统可用资源数
		Finish = new boolean[M];	//标记进程是否完成
		System.out.println("请输入系统初始可用资源数");
		for(int i=0; i<N; i++)//系统初始可用资源数
			All[i] = input.nextInt();
		System.out.println("请输入"+M+"个进程对"+N+"类资源的最大需求量");
		for(int i=0; i<M; i++){//m个进程对n类资源的最大需求量
			ID[i] = i;
			for(int j=0; j<N; j++)
				Max[i][j] = input.nextInt();
		}
		System.out.println("请输入"+M+"个进程已经得到的"+N+"类资源的资源量");
		for(int i=0; i<M; i++)	//m个进程已经得到n类资源的资源量
			for(int j=0; j<N; j++)
				Allocation[i][j] = input.nextInt();
		Need_Resources();
		Available_Resources();
		Print_Banker();
	}

	private void Need_Resources() {//初始化Need矩阵
		// TODO Auto-generated method stub
		for(int i=0; i<M; i++)//m个进程还需要n类资源的资源量
			for(int j=0; j<N; j++)
				Need[i][j] = Max[i][j] - Allocation[i][j];
	}

	private void Available_Resources() {//更新系统当前可用资源数
		// TODO Auto-generated method stub
		for(int n=0; n<N; n++){//系统目前可用资源数
			Available[a][n] = All[n];
			for(int m=0; m<M; m++){
				Available[a][n] -= Allocation[m][n];
			}
		}
	}

	private void Print_Banker() {//T0时刻的资源分配图
		System.out.println("\nT0时刻资源分配图：");
		System.out.print("资源\t资源数量\n");
		for(int i=0; i<N; i++)
			System.out.printf("S%d\t%d\n", i, All[i]);
		System.out.print("\n进程\t Max\tAllocation\tNeed\tAvailable\tFinish\n");
		for(int i=0; i<M; i++){
			System.out.print("P"+ID[i]);
			System.out.print("\t");
			for(int j=0; j<Max[ID[i]].length; j++){
				System.out.printf("%d ", Max[ID[i]][j]);
			}
			System.out.print("\t");
			for(int j=0; j<Allocation[ID[i]].length; j++){
				System.out.printf("%d ", Allocation[ID[i]][j]);
			}
			System.out.print("\t\t");
			for(int j=0; j<Need[ID[i]].length; j++){
				System.out.printf("%d ", Need[ID[i]][j]);
			}
			System.out.print("\t");
			if(i == 0){
				for(int j=0; j<N; j++){
					System.out.printf("%d ", Available[i][j]);
				}
			}
			System.out.print("\t");
			System.out.print("\t");
			System.out.print(Finish[i]);
			System.out.println();
		}
	}

	public void Security_examine(){//安全性检测
		boolean flag1,	//所有进程
				flag2;	//每个进程
		flag1 = true;
		while(flag1){
			flag1 = false;
			for(int i=0; i<M; i++){
				flag2 = true;
				for(int j=0; flag2 && j<N; j++){
					if(Need[i][j] > Available[a][j] || Finish[i]){//存在一个条件不满足或者该进程已经完成
						flag2 = false;
					}
				}
				if(flag2 && !Finish[i]){//该进程（第i个进程）可执行
					flag1 = true;
					Finish[i] = true;
					ID[a] = i;
					a++;//以此判断所有进程是否都执行，安全检查
					for(int j=0; flag2 && j<N; j++){
						Available[a][j] = Available[a-1][j] + Allocation[i][j];
					}
				}
			}
		}
		System.out.println("\nT0时刻的安全性：");
		if(a == M){
			Print_Banker_Se();
			System.out.println("\n安全序列：");
			for(int i=0; i<M; i++){
				System.out.print("->P"+ID[i]);
			}
			System.out.println("\n");
		}
		else{
			System.out.println("系统不安全");
		}
	}
	
	public void Reallocation(){//申请资源
		@SuppressWarnings("resource")
		Scanner input = new Scanner(System.in);
		int[] Request = new int[N];
		boolean flag = true;
		System.out.print("\n输入进程代号：");
		int n = input.nextInt();
		System.out.print("输入请求资源数：");
		for(int j=0; j<N; j++)
			Request[j] = input.nextInt();
		for(int i=0; i<N; i++){//合理性检查,可用性检查
			if(Request[i] > Need[n][i] || Request[i] > Available[0][i])
				flag = false;
		}
		if(flag){
			for(int i=0; i<N; i++){
				Allocation[n][i] += Request[i];
			}
			Init();
			Print_Banker();
		}
		else{
			System.out.println("分配不安全\n");
		}	
	}

	private void Init() {
		// TODO Auto-generated method stub
		a = 0;
		Need_Resources();//再次初始化
		Available_Resources();
		for(int i=0; i<M; i++){//ID初始化
			ID[i] = i;
			Finish[i] = false;
		}
	}

	private void Print_Banker_Se() {
		System.out.print("\n进程\t Work\tNeed\tAllocation\tWork+Allocation\tFinish\n");
		for(int i=0; i<M; i++){
			System.out.print("P"+ID[i]);
			System.out.print("\t");
			for(int j=0; j<Available[i].length; j++){
				System.out.printf("%d ", Available[i][j]);
			}
			System.out.print("\t");
			for(int j=0; j<Need[ID[i]].length; j++){
				System.out.printf("%d ", Need[ID[i]][j]);
			}
			System.out.print("\t");
			for(int j=0; j<Allocation[ID[i]].length; j++){
				System.out.printf("%d ", Allocation[ID[i]][j]);
			}
			System.out.print("\t\t");
			for(int j=0; j<Available[i+1].length; j++){
				System.out.printf("%d ", Available[i+1][j]);
			}
			System.out.print("\t");
			System.out.print("\t");
			System.out.print(Finish[i]);
			System.out.println();
		}
	}	
}
```