---
title: "选择排序(golang实现)"
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
//交换数组元素
func swap(slice []int, i int, j int) {
	slice[i], slice[j] = slice[j], slice[i]
}
 
//选择排序
func SelectionSort(s []int)  {
	l := len(s) //以免每次循环判断都运算
	m := len(s)-1
	for i:=0;i<m;i++ {
		k:=i
		for j:=i+1;j<l;j++ {
			if s[j]<s[k] {
				k = j
			}
		}
		if k!=i {
			swap(s,k,i)
		}
	}
}
```

测试：
``` go
s := []int{9,0,6,5,8,2,1,7,4,3}
fmt.Println(s)
SelectionSort(s)
fmt.Println(s)
```
输出：

[9 0 6 5 8 2 1 7 4 3]  
[0 1 2 3 4 5 6 7 8 9]
