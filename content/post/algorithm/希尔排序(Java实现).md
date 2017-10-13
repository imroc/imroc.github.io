---
title: "希尔排序(Java实现)"
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

封装成类：
``` java
package com.roc.algorithms.sort;

/**
 * 希尔排序
 *
 * @author imroc
 */
public class ShellSort {
    //交换数组元素
    private static void swap(int[] a, int i, int j) {
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    public static void sort(int[] a) {
        int h = 1;
        while (h < a.length / 3) {//寻找合适的间隔h
            h = 3 * h + 1;
        }
        while (h >= 1) {
            //将数组变为间隔h个元素有序
            for (int i = h; i < a.length; i++) {
                //间隔h插入排序
                for (int j = i; j >= h && a[j] < a[j - h]; j -= h) {
                    swap(a, j, j - h);
                }
            }
            h /= 3;
        }
    }
}
```
测试：
``` go
int[] a = {9,0,6,5,8,2,1,7,4,3};
System.out.println(Arrays.toString(a));
ShellSort.sort(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
