---
title: 算法 （一） 选择排序
date: 2019-07-16 22:42:28
categories: "algorithms"
tags:
	- algorithms
---
选择排序，两个for循环嵌套，内部for循环用来找到最大（小）的元素，外部循环用来放置找到的元素。需要遍历数组才能找到峰值元素，所以复杂度与原始序列是否有序无关，最好最坏和平均情况的时间复杂度都为O(n^2);需要一个临时变量用来交换数组内数据位置，所以空间复杂度为O(1)。
<!-- more -->

## 选择排序结果图
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/algorithm/select/selectsort.png)

## 实现
这里用C++来展示
```cpp
#include <cstdio>
#include <Windows.h>

int SelectMax(int arr[])
{
	int length = sizeof(arr);//取得数组长度
	if (length <= 1)
	{
		return arr[0];
	}
	else
	{
		int max = arr[0];//假定第一个最大
		for (int i = 0; i < length; i++)
		{
			if (arr[i] > max)
			{
				max = arr[i];
			}
		}
		return max; 
	}

}

void SelectSort(int *arr, unsigned int length)
{
	if (length <= 1)
	{
		return;
	}
	else
	{
		printf("\n数据交换过程\n");
		for (unsigned int i = 0; i < length - 1; i++)
		{
			int min = i;//最小值假定为第一个
			
			for (unsigned int j = i + 1; j < length; j++)
			{
				if (arr[min] > arr[j])
				{
					min = j;//存储最小
				}
			}
			if (i != min)
			{
				int temp = arr[i];
				arr[i] = arr[min];//交换最小值
				arr[min] = temp;  //数据交换
			}
			printf("arr[%d] = {", length);
			for (int i = 0; i < length; i++)
			{
				printf("%d ",arr[i]);
				if (i == length - 1)
					printf("}\n");
			}
		}
		printf("------------------------------------------\n\n");
	}
	
}
void main()
{
	int arr[] = { 1,9,2,11,10,7,4,6,5,3,8 };
	int length = sizeof(arr) / sizeof(int);//计算数组大小
	SelectSort(arr, length);
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
选择排序算是排序里面最垃圾的算法了。