---
title: "二分法查找(Java实现)"
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
package com.roc.algorithms.search;

/**
 * 二分法查找
 *
 * @author imroc
 */
public class BinarySearch {

    /**
     * @param a 升序排列的数组
     * @param k 待查找的整数
     * @return 如果查到有就返回对应角标，没有就返回-1
     */
    public static int search(int[] a, int k) {
        int lo = 0, hi = a.length - 1;
        while (lo <= hi) {
            int m = (lo + hi) >> 1;
            if (a[m] < k) {
                lo = m + 1;
            } else if (a[m] > k) {
                hi = m - 1;
            } else {
                return m;
            }
        }
        return -1;
    }
}
```
测试：
``` java
int[] a = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
System.out.println(BinarySearch.search(a, 6));
```
输出：
6
