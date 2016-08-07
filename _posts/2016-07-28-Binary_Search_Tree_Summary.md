---
layout: post
title: Binary Search Tree Summary
date: 2016-07-28
categories: blog
tags: [study]

---

# 二叉搜索树

### 基本操作

search（查找）, minimum（最小值）, maximum（最大值）, predecessor（前驱）, successor（后继）, insert（插入）, delete（删除）.

### 应用

* 字典
* 优先队列

### 时间复杂度

最坏 O(n) ， 平均 O(logn)

# 什么是二叉搜索树(binary search tree)

### 性质

左子树任意节点的 key 小于根节点的 key ，右子树任意节点的 key 大于根节点的 key 。

这个性质允许我们使用中序遍历算法按序输出二叉搜索树中的所有关键字。

{% highlight java %}
Inorder(x) {
    if (x != null) {
        Inorder(x.left);
        print(x.key);
        Inorder(x.right);
    }
}
{% endhighlight %}

# 插入与删除

### 插入

{% highlight java %}
public void insert(Node root, Node newNode) {
    Node pre = null;
    Node iter = root;
    while (iter != null) {
        pre = iter;
        if (newNode.val > iter.val) iter = iter.right;
        else if (newNode.val < iter.val) iter = iter.left;
        else return;
    }
    if (pre == null) {
        root = newNode;
        return;
    }
    if (newNode.val < pre.val) pre.left = newNode;
    else pre.right = newNode;
}
{% endhighlight %}

### 删除

删除节点 z 有三种情况：

* z 没有孩子结点，简单删除，修改父结点即可。
* z 只有一个孩子，孩子提到 z 的位置即可。
* z 有两个孩子，找 z 的后继 y ，让 y 占据 z 的位置，z 的右子树成为 y 的右子树，z 的左子树成为 y 的左子树。

