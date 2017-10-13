---
title: "Union-Find(C++实现实现)"
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
``` cpp
class UnionFind
{
public:
    UnionFind(int n);
    ~UnionFind();
    int Count();
    virtual bool Connected(int p,int q)=0;
    virtual void Union(int p,int q)=0;
protected:
    int *id;
    int sz;
    int num;
};
UnionFind::UnionFind(int n)
{
    this->num = this->sz = n;
    this->id = new int[n];
    for(int i=0; i<n; i++)
    {
        this->id[i] = i;
    }
};
UnionFind::~UnionFind()
{
    delete this->id;
    delete &this->num;
    delete &this->sz;
};

int UnionFind::Count()
{
    return this->num;
};
```

## QuickFind

- a和b进行union的时候，将b及与b连通节点的标记都置为和a的标记一样
- 标记相同的节点是连通的

``` cpp
class QuickFind : public UnionFind
{
public:
    QuickFind(int n);
    bool Connected(int p,int q);
    void Union(int p,int q);
};
QuickFind::QuickFind(int n):UnionFind(n) {};


bool QuickFind::Connected(int p,int q)
{
    return this->id[p] == this->id[q];
};

void QuickFind::Union(int p,int q)
{
    int k = this->id[q];
    for(int i=0; i<this->sz; i++)
    {
        if(this->id[i]==k)
        {
            this->id[i]=this->id[p];
        }
    }
    this->num--;
};
```

## QuickUnion
- 连通的节点形成一棵树，根节点相同

``` cpp
class QuickUnion : public UnionFind
{
public:
    QuickUnion(int n);
    bool Connected(int p,int q);
    void Union(int p,int q);
protected:
    int findRoot(int p);
};
QuickUnion::QuickUnion(int n):UnionFind(n) {};
int QuickUnion::findRoot(int p)
{
    while(p!=this->id[p])
    {
        p = this->id[p];
    }
    return p;
};
bool QuickUnion::Connected(int p,int q)
{
    return this->findRoot(p)==this->findRoot(q);
};
void QuickUnion::Union(int p,int q)
{
    int i = this->findRoot(p);
    int j = this->findRoot(q);
    if (i == j)
    {
        return;
    }
    this->id[j] = this->id[i];
    this->num--;
};
```

## 加权QuickUnion(附带路径压缩优化)

- union的时候小树挂在大树下
- 查询根节点的时候顺便将该节点的父节点直接指向根节点，压缩路径

``` cpp
class WeightedQuickUnion : public QuickUnion
{
public:
    WeightedQuickUnion(int n);
    ~WeightedQuickUnion();
    void Union(int p,int q);
protected:
    int findRoot(int p);
private:
    int *sz;
};
WeightedQuickUnion::WeightedQuickUnion(int n):QuickUnion(n)
{
    this->sz = new int[n];
    for(int i=0; i<n; i++)
    {
        this->sz[i] = 1;
    }
};
WeightedQuickUnion::~WeightedQuickUnion()
{
    delete this->sz;
};
int WeightedQuickUnion::findRoot(int p)
{
    while (p != this->id[p])
    {
        this->id[p] = this->id[this->id[p]];
        p = this->id[p];
    }
    return p;
};
void WeightedQuickUnion::Union(int p,int q)
{
    int i = this->findRoot(p);
    int j = this->findRoot(q);
    if (i == j)
    {
        return;
    }
    if (this->sz[i] < this->sz[j])
    {
        this->id[i] = j;
        this->sz[j] += this->sz[i];
    }
    else
    {
        this->id[j] = i;
        this->sz[i] += this->sz[j];
    }
    this->num--;
};
```
