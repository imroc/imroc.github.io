---
title: "快速排序(golang实现)"
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
//快速排序
func QuickSort(s []int) {
	sort(s, 0, len(s)-1)
}
 
func sort(s []int, lo, hi int) {
	if lo >= hi {
		return
	}
	k := partition(s, lo, hi)
	sort(s, lo, k)
	sort(s, k+1, hi)
}
 
func partition(s []int, lo, hi int) int {
	i, j := lo, hi+1 //将lo作为中间值
	for {
		for {
			i++
			if i == hi || s[i] > s[lo] {
				break
			}
		}
		for {
			j--
			if j == lo || s[j] <= s[lo] {
				break
			}
		}
		if i >= j {
			break
		}
		swap(s, i, j)
	}
	swap(s, lo, j)
	return j
}
 
func swap(s []int, i int, j int) {
	s[i], s[j] = s[j], s[i]
}
```
测试：
``` go
s := []int{9, 0, 6, 5, 8, 2, 1, 7, 4, 3}
fmt.Println(s)
QuickSort(s)
fmt.Println(s)
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
