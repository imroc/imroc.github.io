---
title: "冒泡排序(Java实现)"
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

两种类似的方式：
``` java
/**
 * 冒泡排序
 * @author roc
 */
public class BubbleSort {

    //交换数组元素
    private static void swap(int[] a,int i,int j){
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    //第一种冒泡排序
    public static void sort1(int[] a){
        int max = a.length-1;
        int i,j;
        for(i=0;i<max;i++){
            for(j=0;j<max-i;j++){
                if(a[j+1]<a[j]){
                    swap(a,j,j+1);
                }
            }
        }
    }

    //第二种冒泡排序
    public static void sort2(int[] a){
        int max = a.length-1;
        int i,j;
        for(i=0;i<max;i++){
            for(j=i+1;j<a.length;j++){
                if(a[j]<a[i]){
                    swap(a,i,j);
                }
            }
        }
    }
}
```
测试：
``` java
int[] a = {9,0,6,5,8,2,1,7,4,3};
System.out.println(Arrays.toString(a));
BubbleSort.sort1(a);
//BubbleSort.sort2(a);
System.out.println(Arrays.toString(a));
```
输出：

[9, 0, 6, 5, 8, 2, 1, 7, 4, 3]  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
