---
title: "线性复杂度选出第k小元素、中位数、最小的k个元素(Java实现)"
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
package com.roc.algorithms.common;

import java.util.Arrays;

/**
 * 选出第k小元素、中位数、最小的k个元素(线性复杂度)
 *
 * @author imroc
 */
public class Select {


    //选出第k小元素，k为1~a.length
    public static int selectKthMin(int[] a, int k) {
        k--;
        int lo = 0, hi = a.length - 1;
        while (true) {
            int j = partition(a, lo, hi);
            if (j < k) {
                lo = j + 1;
            } else if (j > k) {
                hi = j - 1;
            } else {
                return a[k];
            }
        }
    }

    //选出中位数（比一半的元素小，比另一半的大）
    public static int selectMid(int[] a) {
        return selectKthMin(a, a.length / 2);
    }

    //选出k个最小元素，k为1~a.length
    public static int[] SelectKMin(int[] a, int k) {
        int lo = 0, hi = a.length - 1;
        while (true) {
            int j = partition(a, lo, hi);
            if (j < k) {
                lo = j + 1;
            } else if (j > k) {
                hi = j - 1;
            } else {
                return Arrays.copyOf(a, k);
            }
        }
    }

    private static int partition(int[] a, int lo, int hi) {
        int i = lo, j = hi + 1;
        while (true) {
            while (true) {
                i++;
                if (i == hi || a[i] > a[lo]) {
                    break;
                }
            }
            while (true) {
                j--;
                if (j == lo || a[j] <= a[lo]) {
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
int[] a = { 9, 0, 6, 5, 8, 2, 1, 7, 4, 3};
System.out.println(Select.selectKthMin(a,1));//第1小元素：0
System.out.println(Select.selectMid(a));//中位数：4
System.out.println(Arrays.toString(Select.SelectKMin(a,5)));//最小的5个数：0~4
```
输出：  
0  
4  
[0, 1, 2, 3, 4]
