---
title: "golang中append函数返回值必须有变量接收的原因探究"
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

append函数返回更新后的slice（长度和容量可能会变），必须重新用slice的变量接收，不然无法编译通过
    
为了弄明白为什么，首先我们需要清楚几件事：

- slice的底层是数组，一片连续的内存，slice变量只是存储该slice在底层数组的起始位置、结束位置以及容量。
- 它的长度可以通过起始位置和结束位置算出来，容量也可以通过起点位置到底层数组的末端位置的长度算出来，多个slice可以指向同一个底层数组。所以slice和数组指针不同，数组指针主要存储底层数组的首地址。
- 因为Go函数传递默认是值拷贝，将slice变量传入append函数相当于传了原slice变量的一个副本，注意不是拷贝底层数组，因为slice变量并不是数组，它仅仅是存储了底层数组的一些信息。


所以说，当它改变传入的slice变量的信息，原slice变量并不会有任何变化，打印原slice变量和之前也会一模一样。该函数会返回修改后的slice变量，因为原slice并不会变，假如没有任何slice变量接收返回的值，那么此次append操作就没有意义了。所以必须要有slice变量重新接收修改后的slice变量，不然编译器会报错。Go不希望你做无意义的事，就像导入的包或定义的变量没有用上，它也会报错。


个人是这样理解的，如有不对之处还请指正。
