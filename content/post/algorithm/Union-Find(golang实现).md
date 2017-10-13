---
title: "Union-Find(golang实现)"
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

用一个包专门处理union-find算法(unionfind)

定义接口和基类(`union_find.go`)：

``` go
package unionfind
 
//union-find的quick-find实现版
type QuickFind struct {
	BaseUnionFind
}
 
func (qf *QuickFind) Connected(p, q int) bool {
	return qf.id[p] == qf.id[q]
}
 
func (qf *QuickFind) Union(p, q int) {
	i, j := qf.id[p], qf.id[q]
	if i == j {
		return
	}
	for k, v := range qf.id {
		if v == j {
			qf.id[k] = i
		}
	}
	qf.count--
}
 
func NewQuickFind(n int) *QuickFind {
	return &QuickFind{*NewBaseUnionFind(n)}
}
```

## QuickFind

- a和b进行union的时候，将b及与b连通节点的标记都置为和a的标记一样
- 标记相同的节点是连通的
`quick_find.go:`

``` go
package unionfind
 
//union-find的quick-find实现版
type QuickFind struct {
	BaseUnionFind
}
 
func (qf *QuickFind) Connected(i, j int) bool {
	return qf.id[i] == qf.id[j]
}
 
func (qf *QuickFind) Union(i, j int) {
	c := qf.id[j]
	for k, v := range qf.id {
		if v == c {
			qf.id[k] = qf.id[i]
		}
	}
	qf.count--
}
 
func NewQuickFind(n int) *QuickFind {
	return &QuickFind{*NewBaseUnionFind(n)}
}
```
## QuickUnion

- 连通的节点形成一棵树，根节点相同
`quick_union.go:`

``` go
package unionfind
 
//union-find的quick-union实现版
type QuickUnion struct {
	BaseUnionFind
}
 
//查询根节点
func (qu *QuickUnion) findRoot(p int) int {
	for p != qu.id[p] {
		p = qu.id[p]
	}
	return p
}
 
func (qu *QuickUnion) Connected(p, q int) bool {
	return qu.findRoot(p) == qu.findRoot(q)
}
 
func (qu *QuickUnion) Union(p, q int) {
	rp := qu.findRoot(p)
	rq := qu.findRoot(q)
	if rp == rq {
		return
	}
	qu.id[rq] = rp
	qu.count--
}
 
func NewQuickUnion(n int) *QuickUnion {
	return &QuickUnion{*NewBaseUnionFind(n)}
}
```

## 加权QuickUnion(附带路径压缩优化):

- union的时候小树挂在大树下
- 查询根节点的时候顺便将该节点的父节点直接指向根节点，压缩路径
`weighted_quick_union.go:`

``` go
package unionfind
 
 
//union-find的加权quick-union实现版，
//另外还作了路径压缩优化
type WeightedQuickUnion struct {
	QuickUnion
	sz []int
}
 
//查询根节点，顺便压缩路径
func (wqu *WeightedQuickUnion) findRoot(p int) int {
	for p != wqu.id[p] {
		wqu.id[p] = wqu.id[wqu.id[p]]
		p = wqu.id[p]
	}
	return p
}
 
func (wqu *WeightedQuickUnion) Union(p, q int) {
	rp := wqu.findRoot(p)
	rq := wqu.findRoot(q)
	if rp == rq {
		return
	}
	if wqu.sz[rp] < wqu.sz[rq] {
		wqu.id[rp] = rq
		wqu.sz[rq] += wqu.sz[rp]
	} else {
		wqu.id[rp] = rq
		wqu.sz[rp] += wqu.sz[rq]
	}
	wqu.count--
}
 
func NewWeightedQuickUnion(n int) *WeightedQuickUnion {
	sz := make([]int, n)
	for i := 0; i < n; i++ {
		sz[i] = 1
	}
	return &WeightedQuickUnion{QuickUnion:*NewQuickUnion(n), sz: sz}
}
```
