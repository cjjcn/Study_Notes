---
title: C语言中的动态数组
tags:
  - C
  - 指针
abbrlink: 83e532aa
date: 2021-03-14 09:27:04
---

## 什么是动态数组

这里先引入一段对动态数组的说法

>动态数组是相对于静态数组而言，从“动”字我们也可以看出它的灵活性，静态数组的长度是预先定义好的，在整个程序中，一旦给定大小后就无法改变。而动态数组则不然，它可以随程序需要而重新指定大小。动态数组的内存空间是从堆动态分配的。是通过执行代码而为其分配存储空间。当程序执行到我们编写的分配语句时，才为其分配。对于静态数组，其创建非常方便，使用完也无需释放，要引用也简单，但是创建后无法改变其大小是其致命弱点！对于动态数组，其创建麻烦，使用完必须由程序员自己释放，否则将会引起内存泄露。但其使用非常灵活，能根据程序需要动态分配大小。所以相对于静态数组的来说我们对于使用动态数组有很大的自由度。

同时，动态数组的创建还需要遵循一个原则，就是在创建的时候从外层往里层，逐层创建，释放的时候相反，是从里往外释放。

## 一维动态数组的创建

```c
#include <stdio.h>
#include <stdlib.h>
int main()
{
int n1,i;
int *array;
printf("请输入所要创建的一维动态数组的长度：");
scanf("%d",&n1);
array=(int*)calloc(n1,sizeof(int));
for(i=0;i<n1;i++)
{
 printf("%d\t",array[i]);
}
printf("\n");
for(i=0;i<n1;i++)
{
 array[i]=i+1;
 printf("%d\t",array[i]);
}
 free(array);//释放第一维指针 
return 0;
}
```

## 二维动态数组的创建

```c
#include <iostream>
#include <stdlib.h>
using namespace std;
 
int main(){
	int num1,num2;
	cout<<"请输入动态二维数组的第一个维度：";
	cin>>num1;
	cout<<"请输入动态二维数组的第二个维度：";
	cin>>num2;
	int **array = (int **)calloc(num1,sizeof(int));
	for(int i=0;i<num1;i++) {
		array[i] = (int*)calloc(num2,sizeof(int));
	}
	for(int i=0;i<num1;i++){
		for(int j=0;j<num2;j++){
			array[i][j] =i*num2+j+1;
			printf("%d\t",array[i][j]);
		}
		cout<<endl;
	}
	for(int i=0;i<num1;i++)	free(array[i]);
	free(array);
	return 0;
}
```

## 总结

综上，我们便可以知道如何创建动态数组，以二维动态数组为例，先从外层往里层，逐层创建

```c
int **array=calloc(num1,sizeof(int*)); //第一维
```

然后接着创建第二层

```c
array[i]=calloc(num2 ,sizeof(int));//第二维
```

销毁的时候相反，从里往外逐层销毁，先来释放里层

```c
for(i=0;i<n1;i++) 
{ 
	free(array[i]);//释放第二维指针 
}
```

接着释放外层

```c
free(array);//释放第一维指针
```

