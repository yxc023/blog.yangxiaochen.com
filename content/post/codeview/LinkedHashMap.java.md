+++
date = "2016-03-29T22:19:32+08:00"
draft = false
title = "LinkedHashMap.java(挖坑)"
categories = [ "codeview" ]
type="codeview"
+++

## 概要
### 前置基本知识
数据结构: 链表, 哈希表(散列表), 二叉查找树, 红黑树(optional)

### 名词
1. 桶, 槽: 散列位置.
2. Entry, Node, TreeNode: Entry为一个键值对, Node和TreeNode都是Entry的一种特别类型.

### 结构
#### 基本结构


### 主要方法
1. get()
2. put()
3. remove()
4. keySet()
5. values()

### 进阶
1. 树化
2. 容量策略
3. hash策略
4. resize策略

## 源码注解
