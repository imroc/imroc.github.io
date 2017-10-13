---
title: "二分法查找(C-C++实现)"
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
//二分法查找
//数组a是升序的，len为数组长度
//k为待查找的整数
//如果查到有就返回对应角标,
//没有就返回-1
int search(int *a,int len, int k)
{
    int lo = 0, hi = len - 1;
    while (lo <= hi)
    {
        int m = (lo + hi) >> 1;
        if (a[m] < k)
        {
            lo = m + 1;
        }
        else if (a[m] > k)
        {
            hi = m - 1;
        }
        else
        {
            return m;
        }
    }
    return -1;
}
```
测试：
```
int a[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
int len = sizeof(a)/sizeof(int);
printf("%d\n",search(a,len,6));
```
输出：
6
