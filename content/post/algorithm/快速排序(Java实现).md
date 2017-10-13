---
title: "快速排序(Java实现)"
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
 * 快速排序
 *
 * @author imroc
 */
public class QuickSort {
    public static void sort(int[] a) {
        sort(a, 0, a.length - 1);
    }
    private static void sort(int[] a, int lo, int hi) {
        if (lo >= hi) {
            return;
        }
        int k = partition(a, lo, hi);
        sort(a, lo, k);
        sort(a, k + 1, hi);
    }
    private static int partition(int[] a, int lo, int hi) {
        int i = lo, j = hi + 1;
        while (true) {
            while (a[++i] <= a[lo]) {
                if (i == hi) {
                    break;
                }
            }
            while (a[--j] > a[lo]) {
                if (j == lo) {
                    break;
                }
            }
            if (i >= j) {
                break;
            }
            swap(a, i, j);
        }
        swap(a, lo, j);
        return j;
    }
    private static void swap(int[] a, int i, int j) {
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }
}
```
测试：
``` java
int[] a = {9, 0, 6, 5, 8, 2, 1, 7, 4, 3};
System.out.println(Arrays.toString(a));
QuickSort.sort(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
