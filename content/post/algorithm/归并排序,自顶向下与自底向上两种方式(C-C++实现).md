---
title: "归并排序,自顶向下与自底向上两种方式(C-C++实现)"
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

封装成函数merge_sort_up_to_down（自顶向下方式）和merge_sort_down_to_up（自底向上方式）：
``` c
#include <malloc.h>

//归并操作
void merge(int *a,int *aux,int lo,int mid,int hi)
{
    int i,j,k;
    for(k=lo; k<=hi; k++)
    {
        aux[k] = a[k];
    }
    i = lo;
    j = mid+1;
    for(k=lo; k<=hi; k++)
    {
        if(i>mid)
        {
            a[k] = aux[j++];
        }
        else if(j>hi)
        {
            a[k] = aux[i++];
        }
        else if(aux[j]<aux[i])
        {
            a[k] = aux[j++];
        }
        else
        {
            a[k] = aux[i++];
        }
    }
}

void sort(int *a,int *aux,int lo,int hi)
{
    int mid;
    if(lo>=hi)
    {
        return;
    }
    mid = (lo+hi)>>1;
    sort(a,aux,lo,mid);
    sort(a,aux,mid+1,hi);
    merge(a,aux,lo,mid,hi);
}

//自顶向下归并排序
void merge_sort_up_to_down(int *a,int len)
{
    int *aux = (int*)malloc(len*sizeof(int));
    sort(a,aux,0,len-1);
}

int min(int i,int j)
{
    if (j < i)
    {
        return j;
    }
    return i;
}

//自底向上归并排序
void merge_sort_down_to_up(int *a,int len)
{
    int *aux = (int*)malloc(len*sizeof(int));
    int sz,lo;
    for (sz = 1; sz < len; sz *= 2)
    {
        for (lo = 0; lo < len-sz; lo += 2 * sz)
        {
            merge(a, aux, lo, lo+sz-1, min(lo+2*sz-1, len-1));
        }
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
    int a[] = {9,0,6,5,8,2,1,7,4,3};
    int len = sizeof(a)/sizeof(int);
    print_arr(a,len);
    merge_sort_up_to_down(a,len);
    //merge_sort_down_to_up(a,len);
    print_arr(a,len);
    return 0;
}
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
