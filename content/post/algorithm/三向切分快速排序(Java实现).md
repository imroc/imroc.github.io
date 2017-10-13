---
title: "三向切分快速排序(Java实现)"
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
 * 三向切分快速排序
 * 适合有较多重复元素的排序算法
 *
 * @author imroc
 */
public class ThreeWayQuickSort {

    public static void sort(int[] a) {
        sort(a, 0, a.length - 1);
    }

    //在lt之前的(lo~lt-1)都小于中间值
    //在gt之前的(gt+1~hi)都大于中间值
    //在lt~i-1的都等于中间值
    //在i~gt的都还不确定（最终i会大于gt，即不确定的将不复存在）
    private static void sort(int[] a, int lo, int hi) {
        if (lo >= hi) {
            return;
        }
        int v = a[lo], lt = lo, i = lo + 1, gt = hi;
        while (i <= gt) {
            if (a[i] < v) {
                swap(a, i++, lt++);
            } else if (a[i] > v) {
                swap(a, i, gt--);
            } else {
                i++;
            }
        }
        sort(a, lo, lt - 1);
        sort(a, gt + 1, hi);
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
ThreeWayQuickSort.sort(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
