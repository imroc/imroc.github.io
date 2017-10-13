---
title: "堆排序(C-C++实现)"
date: 2017-09-05T15:35:18+08:00
lastmod: 2017-09-05T15:35:18+08:00
draft: false
tags: [ "算法" ]

# you can close something for this content if you open it in config.toml.
comment: false
toc: false
# you can define another contentCopyright. e.g. contentCopyright: "This is an another copyright."
contentCopyright: false
reward: false
mathjax: false
---

封装成函数：
``` c
//交换数组元素
void swap(int *a,int i,int j)
{
    int t = a[i];
    a[i] = a[j];
    a[j] = t;
}
//下沉（由上至下的堆有序化）
void sink(int *a,int k,int N)
{
    while (1)
    {
        int i = 2 * k;
        if (i > N)   //保证该节点是非叶子节点
        {
            break;
        }
        if (i < N && a[i + 1] > a[i])   //选择较大的子节点
        {
            i++;
        }
        if (a[k] >= a[i])   //没下沉到底就构造好堆了
        {
            break;
        }
        swap(a, k, i);
        k = i;
    }
}
//堆排序
//a[0]不用，实际元素从角标1开始
//父节点元素大于子节点元素
//左子节点角标为2*k
//右子节点角标为2*k+1
//父节点角标为k/2
void heap_sort(int *a,int len)
{
    int N,k;
    //s[0]不用，实际元素数量和最后一个元素的角标都为N
    N = len - 1;
    //构造堆
    //如果给两个已构造好的堆添加一个共同父节点，
    //将新添加的节点作一次下沉将构造一个新堆，
    //由于叶子节点都可看作一个构造好的堆，所以
    //可以从最后一个非叶子节点开始下沉，直至
    //根节点，最后一个非叶子节点是最后一个叶子
    //节点的父节点，角标为N/2
    for (k = N / 2; k >= 1; k--)
    {
        sink(a, k, N);
    }
    //下沉排序
    while (N > 1)
    {
        swap(a, 1, N--); //将大的放在数组后面，升序排序
        sink(a, 1, N);
    }
}
```
测试：
``` c
#include <stdio.h>

//打印输出数组
void print_arr(int *a,int len)
{
    int i;
    if(len<1)  //数组长度必须大于0
    {
        printf("length greater than 0");
        return;
    }
    //打印整个数组
    printf("[");
    for(i=0; i<len-1; i++)
    {
        printf("%d ",a[i]);
    }
    printf("%d]\n",a[len-1]);
}

int main()
{
    int a[] = {-1,9,0,6,5,8,2,1,7,4,3};
    int len = sizeof(a)/sizeof(int);
    print_arr(&a[1],len-1);
    heap_sort(a,len);
    print_arr(&a[1],len-1);
    return 0;
}
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
