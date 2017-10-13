---
title: "线性复杂度选出第k小元素、中位数、最小的k个元素(C-C++实现)"
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
#include <malloc.h>

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

int* copy_of(int *a,int len)
{
    int *ret = (int*)malloc(sizeof(int)*len);
    int i;
    for(i=0; i<len; i++)
    {
        ret[i] = a[i];
    }
    return ret;
}

//选出第k小元素，k为1~len
int select_kth_min(int *a, int len,int k)
{
    k--;
    int lo = 0, hi = len - 1;
    while (1)
    {
        int j = partition(a, lo, hi);
        if (j < k)
        {
            lo = j + 1;
        }
        else if (j > k)
        {
            hi = j - 1;
        }
        else
        {
            return a[k];
        }
    }
}

//选出中位数（比一半的元素小，比另一半的大）
int select_mid(int *a,int len)
{
    return select_kth_min(a, len, len / 2);
}

//选出k个最小元素，k为1~len
int* select_k_min(int *a, int len,int k)
{
    int lo = 0, hi = len - 1;
    while (1)
    {
        int j = partition(a, lo, hi);
        if (j < k)
        {
            lo = j + 1;
        }
        else if (j > k)
        {
            hi = j - 1;
        }
        else
        {
            return copy_of(a, k);
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
    printf("%d\n",select_kth_min(a,len,1));//第1小元素：0
    printf("%d\n",select_mid(a,len));//中位数：4
    print_arr(select_k_min(a,len,5),5);//最小的5个数：0~4
    return 0;
}
```
输出：  
0  
4  
[0 1 2 3 4]
