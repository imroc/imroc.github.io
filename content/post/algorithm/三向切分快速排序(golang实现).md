---
title: "三向切分快速排序(golang实现)"
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
//三向切分快速排序
func ThreeWayQuickSort(s []int) {
	sort3way(s, 0, len(s)-1)
}
 
//在lt之前的(lo~lt-1)都小于中间值
//在gt之前的(gt+1~hi)都大于中间值
//在lt~i-1的都等于中间值
//在i~gt的都还不确定（最终i会大于gt，即不确定的将不复存在）
func sort3way(s []int, lo, hi int) {
	if lo >= hi {
		return
	}
	v, lt, i, gt := s[lo], lo, lo+1, hi
	for i <= gt {
		if s[i] < v {
			swap(s, i, lt)
			lt++
			i++
		} else if s[i] > v {
			swap(s, i, gt)
			gt--
		} else {
			i++
		}
	}
	sort3way(s, lo, lt-1)
	sort3way(s, gt+1, hi)
}
 
func swap(s []int, i int, j int) {
	s[i], s[j] = s[j], s[i]
}
```
测试：
``` go
s := []int{9, 0, 6, 5, 8, 2, 1, 7, 4, 3}
fmt.Println(s)
ThreeWayQuickSort(s)
fmt.Println(s)
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
