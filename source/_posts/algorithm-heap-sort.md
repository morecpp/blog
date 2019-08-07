---
title: 算法 （二） 堆排序
date: 2019-07-17 22:08:31
categories: "algorithms"
tags:
	- algorithms
---
（**二叉**）**堆**是一个数组，它可以被看成一个近似的完全二叉树。堆排序的时间复杂度是**O(nlgn)**。堆排序具有空间原址性质，任何时候只要常数个额外的元素空间存储临时数据。<!-- more -->
## 二叉树和数组的展示
如下图所示,二叉树上的每一个节点对应数组的一个元素。除了最底层外，该树是完全充满的，而且是从左向右填充。父节点数组下标总在左孩子节点跟右孩子的节点的左边。在大多数操作中，通过将下标i的值左移1位，这样跟数组下标array[0]对应起来了。可以得到一个数学关系。parent(i),left(2\*i),right(2\*i + 1);即left = 2 \* parent，right = 2 \* parent + 1；当然还可以右移动下标1位，总之就是为了让它得到一个左右孩子跟父节点的数学关系式。
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/algorithm/heap/heaptree.png)
## 最大堆
其实就是相当于数组里面的波峰值，把数组的下标想象成细胞。类似细胞分裂，一段时间内一一个细胞只能分裂成两个，1个变2个，2个变4个,4个变8个...原始母细胞指向的值要大于所有它分裂出来的细胞指向的值。
## 最小堆
同理，相当于数组里面的波谷值，原始母细胞指向的值最小。
## 堆排序过程跟结果
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/algorithm/heap/heapsort.png)
## 代码展示
```cpp
#include <cstdio>
#include <Windows.h>

int* HeapSortMax(int *arr,unsigned int length)//求最大值，最大值放到arr[0]
{
	if (length <= 1)
	{
		return arr;
	}
	else
	{
		unsigned int depth = length / 2 - 1;//二叉树的深度，遍历所有中央节点
		unsigned int topmax;    //最大值的下标
		unsigned int leftchild; //左孩子的下标
		unsigned int rightchild;//右孩子的下标
		printf("\n数据交换过程\n");
		for (int i = depth; i >= 0; i--)
		{
			topmax = i;//假定最大的值在下标i的位置
			leftchild = 2 * i;     //左节点
			rightchild = 2 * i + 1;//有节点
			if (leftchild <= length - 1 && arr[leftchild] > arr[topmax])//防止越界
			{
				topmax = leftchild;
			}
			if (rightchild <= length - 1 && arr[rightchild] > arr[topmax])//防止越界
			{
				topmax = rightchild;
			}
			if (topmax != i)
			{
				//数据交换
				int temp = arr[i];
				arr[i] = arr[topmax];
				arr[topmax] = temp;
			}
			printf("arr[%d] = {", length);
			for (unsigned int i = 0; i < length; i++)
			{
				printf("%d ", arr[i]);
				if (i == length - 1)
					printf("}\n");
			}
		}
		return arr;//最大值被移动到了头部
	}
}

int *HeapSort(int *arr, int length)
{
	int temp;
	int lastlength = length;
	for (int i = 0; i < length; i++)
	{
		lastlength = length - i;
		HeapSortMax(arr, lastlength);
		if (arr[0] > arr[lastlength - 1])
		{
			temp = arr[lastlength - 1];
			arr[lastlength - 1] = arr[0];
			arr[0] = temp;
		}
	}
	return arr;
}

void main()
{
	int arr[] = { 15,8,4,13,7,6,5,2,9,14 };
	int length = sizeof(arr) / sizeof(int);
	//HeapSortMax(arr, length);
	HeapSort(arr, length);
	printf("最终结果：\n");
	printf("arr[%d] = {", length);
	for (int i = 0; i < length; i++)
	{
		printf("%d ",arr[i]);
	}
	printf("}\n\n\n");
	system("title xiaoyuren");
	system("pause");
}
```