---
title: "选择排序(Java实现)"
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
/**
 * 选择排序
 *
 * @author imroc
 */
public class SelectionSort {
    //交换数组元素
    private static void swap(int[] a, int i, int j) {
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    //选择排序
    public static void sort(int[] a) {
        int m = a.length - 1; //以免每次循环判断都运算
        for (int i = 0; i < m; i++) {
            int k = i;
            for (int j = i + 1; j < a.length; j++) {
                if (a[j] < a[k]) {
                    k = j;
                }
            }
            if (k != i) {
                swap(a, k, i);
            }
        }
    }
}
```
测试：
``` java
int[] a = {9,0,6,5,8,2,1,7,4,3};
System.out.println(Arrays.toString(a));
SelectionSort.sort(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
