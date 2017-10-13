---
title: "希尔排序(C-C++实现)"
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
``` go
//交换数组元素
void swap(int *a,int i,int j)
{
    int t = a[i];
    a[i] = a[j];
    a[j] = t;
}
//希尔排序
void shell_sort(int *a,int len)
{
    int h=1,i,j;
    while(h<len/3)  //寻找合适的间隔h
    {
        h = 3*h+1;
    }
    while(h>=1)
    {
        //将数组变为间隔h个元素有序
        for (i = h; i < len; i++)
        {
            //间隔h插入排序
            for (j = i; j >= h && a[j] < a[j-h]; j -= h)
            {
                swap(a, j, j-h);
            }
        }
        h /= 3;
    }
}
```

测试：
``` go
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
    shell_sort(a,len);
    print_arr(a,len);
    return 0;
}
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
