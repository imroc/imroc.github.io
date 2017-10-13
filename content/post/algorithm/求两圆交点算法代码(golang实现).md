---
title: "求两圆交点算法代码(golang实现)"
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

实现代码：
``` go
package main

import (
	"math"
)

//代表一个点，包含横纵坐标
type Point struct {
	X, Y float64
}

//代表一个圆，包含横纵坐标及半径
type Circle struct {
	Point
	R float64
}

//创建圆对象
func NewCircle(x, y, r float64) *Circle {
	return &Circle{Point{x, y}, r}
}

//求两圆相交的交点，交点个数可能有0,1,2
func Intersect(a *Circle, b *Circle) (p []Point) {
	dx, dy := b.X - a.X, b.Y - a.Y
	lr := a.R + b.R //半径和
	dr := math.Abs(a.R - b.R) //半径差
	ab := math.Sqrt(dx * dx + dy * dy) //圆心距
	if ab <= lr && ab > dr {
		theta1 := math.Atan(dy / dx)
		ef := lr - ab
		ao := a.R - ef / 2
		theta2 := math.Acos(ao / a.R)
		theta := theta1 + theta2
		xc := a.X + a.R * math.Cos(theta)
		yc := a.Y + a.R * math.Sin(theta)
		p = append(p, Point{xc, yc})
		if ab < lr { //两个交点
			theta3 := math.Acos(ao / a.R)
			theta = theta3 - theta1
			xd := a.X + a.R * math.Cos(theta)
			yd := a.Y - a.R * math.Sin(theta)
			p = append(p, Point{xd, yd})
		}
	}
	return
}
```
