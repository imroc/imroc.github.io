---
title: "快速排序(C-C++实现)"
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
//快速排序
void quick_sort(int *a,int len)
{
    sort(a,0,len-1);
}
void sort(int *a,int lo,int hi)
{
    int i,j,k;
    if(lo>=hi)
    {
        return;
    }
    k = partition(a,lo,hi);
    sort(a,lo,k);
    sort(a,k+1,hi);
}
//交换数组元素
void swap(int *a,int i,int j)
{
    int t = a[i];
    a[i] = a[j];
    a[j] = t;
}
int partition(int *a,int lo,int hi)
{
    int i,j;
    i=lo;
    j=hi+1;
    while(1)
    {
        while(a[++i]<=a[lo])
        {
            if(i==hi)
            {
                break;
            }
        }
        while(a[--j]>a[lo])
        {
            if(j==lo)
            {
                break;
            }
        }
        if(i>=j)
        {
            break;
        }
        swap(a,i,j);
    }
    swap(a,lo,j);
    return j;
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
    int a[] = {9,0,6,5,8,2,1,7,4,3};
    int len = sizeof(a)/sizeof(int);
    print_arr(a,len);
    quick_sort(a,len);
    print_arr(a,len);
    return 0;
}
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
