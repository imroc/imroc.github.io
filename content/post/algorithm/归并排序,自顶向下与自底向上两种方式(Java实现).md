---
title: "归并排序,自顶向下与自底向上两种方式(Java实现)"
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
 * 归并排序
 *
 * @author imroc
 */
public class MergeSort {
    //自顶向下方式
    public static void sortUpToDown(int[] a) {
        int[] aux = new int[a.length];
        sort(a, aux, 0, a.length - 1);
    }

    //自底向上方式
    public static void sortDownToUp(int[] a) {
        int[] aux = new int[a.length];
        for (int sz = 1; sz < a.length; sz *= 2) {
            for (int lo = 0; lo < a.length - sz; lo += 2 * sz) {
                merge(a, aux, lo, lo + sz - 1, Math.min(lo + 2 * sz - 1, a.length - 1));
            }
        }
    }

    private static void merge(int[] a, int[] aux, int lo, int mid, int hi) {
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++) {
            if (i > mid) {
                a[k] = aux[j++];
            } else if (j > hi) {
                a[k] = aux[i++];
            } else if (aux[j] < aux[i]) {
                a[k] = aux[j++];
            } else {
                a[k] = aux[i++];
            }
        }
    }

    private static void sort(int[] a, int[] aux, int lo, int hi) {
        if (lo >= hi) {
            return;
        }
        int mid = (lo + hi) >> 1;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }
}
```
测试：
``` java
int[] a = {9, 0, 6, 5, 8, 2, 1, 7, 4, 3};
System.out.println(Arrays.toString(a));
MergeSort.sortUpToDown(a);
//MergeSort.sortDownToUp(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
