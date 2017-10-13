---
title: "Docker容器间通信"
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

- 容器每次启动时会分配个一个IP地址，这个IP地址只在宿主主机内部有用，其它主机上的程序无法访问此IP

- 一台机器上的docker容器之间默认是可以通过分配的IP进行通信的，可以通过启动参数 -icc=false —iptables=true 来关闭互通，严格隔离以提高安全性

- 虽然每次启动分配的IP可能会变，但启动时加类似 —link redis:db 这样的参数给容器起别名，可以起到DNS作用，原理是在 /etc/hosts 里面映射IP到别名，这样你的程序和其他容器通信就可以不用管IP，用别名，IP变但它不会变

- link只支持单主机，跨主机link最早的方案是Ambassador(docker远程代理)，每台主机启动一个Ambassador容器负责对主机上其它容器的网络转发(socket proxy)，后演变成 github.com/gliderlabs/connectable

- 手动处理容器间通信很麻烦，一般用编排工具：kubernetes、swarm......
