---
title: "希尔排序(golang实现)"
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
func ShellSort(a []int) {
	n := len(a)
	h := 1
	for h < n/3 { //寻找合适的间隔h
		h = 3*h + 1
	}
	for h >= 1 {
		//将数组变为间隔h个元素有序
		for i := h; i < n; i++ {
			//间隔h插入排序
			for j := i; j >= h && a[j] < a[j-h]; j -= h {
				swap(a, j, j-h)
			}
		}
		h /= 3
	}
}
 
func swap(slice []int, i int, j int) {
	slice[i], slice[j] = slice[j], slice[i]
}
```
测试：
``` go
s := []int{9,0,6,5,8,2,1,7,4,3}
fmt.Println(s)
ShellSort(s)
fmt.Println(s)
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
