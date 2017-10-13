---
title: "Go语言(golang)包设计哲学-原则与项目结构组织最佳实践"
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


总结下Go的package设计哲学

1. 明确目的
	* 在准备设计一个包之前，我们需要明确它的目的。
	* 包的命名就必须明确体现其目的，而不仅仅是为了存放代码。像标准库的io,http,fmt这些包名就很好，而像util.helper,common这种命名就是反面教材。
2. 可用性
	* 想想使用这个包的人真正的需求，包的使用一定要直观、简单。
	* 在不断迭代开发、优化、完善的时候，不能让引用这个的程序出错。
	* 防止出现需要类型断言具体类型的需求。
	* 让单个包的代码量简化到最少，减少bug，易于掌控。
3. 可移植性
	* 始终追求最高可移植性。
	* 如果包合理实用，就不要过多在意其它人的意见，没有适合所有人的完美的包。
	* 不要让包成为单一依赖点(即所有其它包都依赖它)，每个包都有自己的设计目的，可能多个包会有重复的类型，即便重复定义也不要让包成为单一依赖点，这是API设计原则。



项目结构组织的最佳实践

有两种类型的项目，一种是生成可运行程序的项目(application project)，另一种是专门用于被其它项目引用的套件项目(kit project)。
对于套件项目，结构组织根据实际项目用途而定，而对于可运行程序的项目，用这样的结构：

├── cmd  
├── internal  
└── vendor  

vendor存放依赖包，internal存放本项目内部使用的包，cmd存放可运行的程序的代码，如果该项目有多个可运行程序，可以在cmd下建子目录。

**注:** internal可以被编译器识别，在internal下面的包是不能被其它项目引用的。

internal下面如果有很多包，并且它们需要用到一些相同的逻辑，比如加解密、网络请求等，可以在internal中建platform目录，存放内部使用的套件包，这样的结构：

├── cmd  
├── internal  
│   └── platform  
└── vendor  

`注：platform下面的包如果成熟了，在未来它们你可以将其开源变成公有的套件项目，供别人的项目引用。`

关于日志输出，公有的套件项目不要打印日志，因为它是要被其它项目引用的，打印日志的逻辑应该让可运行的项目自己来决定。


附上相关演讲视频：[《Go语言面向包设计》](https://golangnews.com/stories/2019-video-william-kennedy-package-oriented-design-in-go-gopherconindia)

**插播广告，go的http请求库req，让http请求简单到极致：**https://github.com/imroc/req
