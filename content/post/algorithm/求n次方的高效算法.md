---
title: "求n次方的高效算法"
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

**注：**次幂n为整数，底数可以是整数、小数、矩阵等(只要能进行乘法运算的
 
举个求整数的n次方的例子(Go语言版)：
``` go
func pow(x, n int) int {
	ret := 1 // 结果初始为0次方的值，整数0次方为1。如果是矩阵，则为单元矩阵。
	for n != 0 {
		if n%2 != 0 {
			ret = ret * x
		}
		n /= 2
		x = x * x
	}
	return ret
}

func main() {
	x := pow(2, 10) // 2^10
	println(x)      // 1024
}
```
