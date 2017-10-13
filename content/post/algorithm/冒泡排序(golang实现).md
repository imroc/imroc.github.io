---
title: "冒泡排序(golang实现)"
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

有两种相似的方式：
``` go
func swap(slice []int, i int, j int) {
   slice[i], slice[j] = slice[j], slice[i]
}
 
//第一种冒泡排序
func BubbleSort1(slice []int) {
   length := len(slice)
   max := length - 1
   for i := 0; i < max; i++ {
      for j := 0; j < max-i; j++ {
         if slice[j+1] < slice[j] {
            swap(slice, j, j+1)
         }
      }
   }
}
 
//第二种冒泡排序
func BubbleSort2(slice []int) {
   length := len(slice)
   max := length - 1
   for i := 0; i < max; i++ {
      for j := i + 1; j < length; j++ {
         if slice[j] < slice[i] {
            swap(slice, i, j)
         }
      }
   }
}
```
------
测试：
``` go
slice := []int{9,0,6,5,8,2,1,7,4,3}
fmt.Println(slice)
BubbleSort1(slice)
//BubbleSort2(slice)
fmt.Println(slice)
```
------
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
