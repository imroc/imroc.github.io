---
title: "golang实现二叉查找树"
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

为简单起见，键值均为整型。

定义接口(tree.go)：
``` go
type Tree interface {
	Put(k, v int)  //新增或修改
	Get(k int) int //查询
	Delete(k int)  //删除
	Size() int     //树的大小
	Min() int      //最小键
	DeleteMin()    //删除最小键
}
```
二叉查找树(binary_tree.go)：
``` go
//二叉查找树
type BinaryTree struct {
	root *node
	n    int
}
 
//创建节点
func newNode(k, v int) *node {
	return &node{k: k, v: v, sz: 1}
}
 
//创建二叉查找树
func NewBinaryTree() *BinaryTree {
	return &BinaryTree{}
}
 
//增加或修改
func (bt *BinaryTree) Put(k, v int) {
	bt.root, _ = put(bt.root, k, v)
}
 
//查找
func (bt *BinaryTree) Get(k int) int {
	return get(bt.root, k)
}
 
//树的大小
func (bt *BinaryTree) Size() int {
	return size(bt.root)
}
 
//选出最小键
func (bt *BinaryTree) Min() int {
	return min(bt.root).k
}
 
//删除最小键
func (bt *BinaryTree) DeleteMin() {
	bt.root = deleteMin(bt.root)
}
 
//删除
func (bt *BinaryTree) Delete(k int) {
	bt.root = delete(bt.root, k)
}
```
节点(node.go)：
``` go
//节点
type node struct {
	k, v, sz    int   //键，值，大小
	left, right *node //左右子节点
}

//在以nd为根节点的树下增加或修改一个节点
//如果创建了新节点，第二个参数返回true，
//如果只是修改，第二个参数返回false
func put(nd *node, k, v int) (*node, bool) {
	if nd == nil {
		return newNode(k, v), true
	}
	hasNew := false //检测是否创建了新节点
	if k < nd.k {
		nd.left, hasNew = put(nd.left, k, v)
	} else if k > nd.k {
		nd.right, hasNew = put(nd.right, k, v)
	} else {
		nd.v = v //仅修改，不会增加增加节点，就不更新节点大小
	}
	if hasNew { //如果创建了新节点就更新树的大小
		updateSize(nd)
	}
	return nd, hasNew
}
 
//在以nd为根节点的树中获取键为k的值
func get(nd *node, k int) int {
	if nd == nil {
		return -1
	}
	if k < nd.k {
		return get(nd.left, k)
	} else if k > nd.k {
		return get(nd.right, k)
	} else {
		return nd.v
	}
}
 
//获取以nd为根节点的树的大小
func size(nd *node) int {
	if nd == nil {
		return 0
	}
	return nd.sz
}
 
//更新以nd为根节点的树的大小
func updateSize(nd *node) {
	if nd == nil {
		return
	}
	nd.sz = size(nd.left) + size(nd.right) + 1
}
 
//选出以nd为根节点的树的最小键节点
func min(nd *node) *node {
	if nd == nil {
		return nil
	}
	if nd.left != nil {
		return min(nd.left)
	}
	return nd
}
 
//删除以nd为根节点的树的最小键节点
//返回被删除的节点
func deleteMin(nd *node) *node {
	if nd == nil {
		return nil
	}
	if nd.left == nil { //找到最小节点
		nd = nd.right //用右子节点代替自己
	} else { //还有更小的
		nd.left = deleteMin(nd.left)
	}
	updateSize(nd)
	return nd
}
 
//删除以nd为根节点的树并且键为k的节点
func delete(nd *node, k int) *node {
	if nd == nil {
		return nil
	}
	if k < nd.k {
		nd.left = delete(nd.left, k)
	} else if k > nd.k {
		nd.right = delete(nd.right, k)
	} else { //命中要删除的节点
		//只有一个或零个子节点
		if nd.right == nil {
			return nd.left
		}
		if nd.left == nil {
			return nd.right
		}
		//同时具有两个子节点
		//先找出大于本节点的最小节点作为后继节点
		t := nd
		nd.k = min(t.right).k
		//删除
		deleteMin(t.right)
		//用后继节点代替本节点
		nd.left = t.left
	}
	updateSize(nd)
	return nd
}
```
