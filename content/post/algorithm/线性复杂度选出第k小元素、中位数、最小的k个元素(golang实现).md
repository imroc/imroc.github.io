---
title: "线性复杂度选出第k小元素、中位数、最小的k个元素(golang实现)"
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
//选出第k小元素，k为1~len(s)
func SelectKthMin(s []int, k int) int {
	k--
	lo, hi := 0, len(s)-1
	for {
		j := partition(s, lo, hi)
		if j < k {
			lo = j + 1
		} else if j > k {
			hi = j - 1
		} else {
			return s[k]
		}
	}
}
 
//选出中位数（比一半的元素小，比另一半的大）
func SelectMid(s []int) int {
	return SelectKthMin(s, len(s)/2)
}
 
//选出k个最小元素，k为1~len(s)
func SelectKMin(s []int, k int) []int {
	lo, hi := 0, len(s)-1
	for {
		j := partition(s, lo, hi)
		if j < k {
			lo = j + 1
		} else if j > k {
			hi = j - 1
		} else {
			return s[:k]
		}
	}
}
 
func partition(s []int, lo, hi int) int {
	i, j := lo, hi+1
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
fmt.Println(SelectKthMin(s,1)) //第1小元素：0
fmt.Println(SelectMid(s)) //中位数：4
fmt.Println(SelectKMin(s,5)) //最小的5个数：0~4
```
输出：  
0  
4  
[0 1 2 3 4]
