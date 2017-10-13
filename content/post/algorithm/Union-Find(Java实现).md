---
title: "Union-Find(Java实现)"
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

**quick-find、quick-union、加权quick-union(附带路径压缩优化)**

------
本算法主要解决动态连通性一类问题，这里尽量用精炼简洁的话来阐述。

**数据结构描述:**

- 有N个节点（索引0~N-1），可以查询节点数量
- 可以连接两个节点
- 可以查询两个节点是否连通

**算法大致设计思路：**

- 每个节点初始化为不同的整数标记
- 通过一个辅助函数查询某个节点的标记值
- 如果两节点标记相同，说明两节点是连通的
------

抽象基类：

``` java
package com.roc.algorithms.unionfind;

/**
 * Union-Find算法的基类
 * @author roc
 */
public abstract class UnionFind {
    protected int[] id;
    protected int count;
    public UnionFind(int n){
        this.count = n;
        this.id = new int[n];
        for (int i=0;i<n;i++){
            this.id[i] = i;
        }
    }
    public int getCount(){
        return this.count;
    }
    public abstract boolean isConnected(int p,int q);
    public abstract void union(int p,int q);
}
```

## QuickFind

- a和b进行union的时候，将b及与b连通节点的标记都置为和a的标记一样
- 标记相同的节点是连通的

``` java
package com.roc.algorithms.unionfind;

/**
 * union-find算法的quick-find实现版本
 *
 * @author roc
 */
public class QuickFind extends UnionFind {
    public QuickFind(int n) {
        super(n);
    }

    @Override
    public boolean isConnected(int p, int q) {
        return id[p] == id[q];
    }

    @Override
    public void union(int p, int q) {
        int i = id[p];
        int j = id[q];
        if (i == j) {
            return;
        }
        for (int k = 0; k < id.length; k++) {
            if (id[k] == j) {
                id[k] = i;
            }
        }
        count--;
    }
}
```

## QuickUnion
- 连通的节点形成一棵树，根节点相同

``` java
package com.roc.algorithms.unionfind;

/**
 * union-find算法的quick-union实现版本
 * @author roc
 */
public class QuickUnion extends UnionFind {
    public QuickUnion(int n) {
        super(n);
    }

    private int findRoot(int p) {
        while (p != id[p]) {
            p = id[p];
        }
        return p;
    }

    @Override
    public boolean isConnected(int p, int q) {
        return findRoot(p) == findRoot(q);
    }

    @Override
    public void union(int p, int q) {
        int i = findRoot(p);
        int j = findRoot(q);
        if (i == j) {
            return;
        }
        id[j] = id[i];
        count--;
    }
}
```

## 加权QuickUnion(附带路径压缩优化)

- union的时候小树挂在大树下
- 查询根节点的时候顺便将该节点的父节点直接指向根节点，压缩路径

``` java
package com.roc.algorithms.unionfind;

/**
 * union-find算法的加权quick-union实现版本，
 * 附带路径压缩优化
 *
 * @author roc
 */
public class WeightedQuickUnion extends QuickUnion {
    private int[] sz;

    public WeightedQuickUnion(int n) {
        super(n);
        this.sz = new int[n];
        for (int i = 0; i < n; i++) {
            this.sz[i] = 1;
        }
    }

    ////查询根节点，顺便压缩路径
    @Override
    protected int findRoot(int p) {
        while (p != id[p]) {
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }

    @Override
    public void union(int p, int q) {
        int i = findRoot(p);
        int j = findRoot(q);
        if (i == j) {
            return;
        }
        if (sz[i] < sz[j]) {
            id[i] = j;
            sz[j] += sz[i];
        } else {
            id[j] = i;
            sz[i] += sz[j];
        }
        count--;
    }
}
```
