---
title: "Docker中CMD与ENTRYPOINT的简明理解"
date: 2017-09-06T16:35:18+08:00
lastmod: 2017-09-06T16:35:18+08:00
draft: false
tags: [ "docker" ]

# you can close something for this content if you open it in config.toml.
comment: false
toc: false
# you can define another contentCopyright. e.g. contentCopyright: "This is an another copyright."
contentCopyright: false
reward: false
mathjax: false
---

`CMD`提供容器启动的默认行为，运行不指定运行的命令及参数，会默认执行CMD中的。

例如hello镜像的Dockerfile中有 `CMD ["echo","'hello world'"] `

 - 执行 `docker run hello`，输出`hello world`
 - 执行 `docker run hello /bin/sh -c "shit"` 则会覆盖`CMD`中所有的东西，输出`shit`

------
`ENTRYPOINT`让容器可以当成一个普通的可执行命令一样使用。

例如echo镜像的Dockerfile中有`ENTRYPOINT ["/bin/echo"]`

- 执行`docker run echo "hello world"`，输出`hello world`

若同时`CMD`也存在，比如: `CMD ["'hello world'"]` ，那么`CMD`中的值就是`ENTRYPOINT`的默认参数。

- 执行 `docker run echo`， 即不加参数，也会输出`hello world`
