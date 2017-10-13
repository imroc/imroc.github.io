---
title: "Go语言技巧-使用select{}阻塞main函数"
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

很多时候我们需要让main函数不退出，让它在后台一直执行，例如：
``` go
func main() {
	for i := 0; i < 20; i++ { //启动20个协程处理消息队列中的消息
		c := consumer.New()
		go c.Start()
	}
	select {} // 阻塞
}
```

