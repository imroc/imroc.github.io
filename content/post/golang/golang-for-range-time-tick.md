---
title: "Go语言技巧-使用for range time.Tick()固定间隔时间执行"
date: 2017-09-06T15:35:18+08:00
lastmod: 2017-09-06T15:35:18+08:00
draft: false
keywords: [ "golang", "go" ]
tags: [ "golang" ]

# you can close something for this content if you open it in config.toml.
comment: false
toc: false
# you can define another contentCopyright. e.g. contentCopyright: "This is an another copyright."
contentCopyright: false
reward: false
mathjax: false
---

直接上代码:
``` go
for range time.Tick(30 * time.Millisecond) {
	doSomthing()
}
```

因为time.Tick()返回的是一个channel,每隔指定的时间会有数据从channel中出来，for range不仅能遍历map,slice,array还能取出channel中数据，range前面可以不用变量接收，所以可以简写成上面的形式。

