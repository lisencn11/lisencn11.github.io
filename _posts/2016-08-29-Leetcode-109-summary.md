---
layout: post
title: Leetcode Problem 109 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

# 我的算法

最朴实的算法：将单链表转化为数组，利用 BST 性质做。时间复杂度是 O(n) 但是还需要 O(n) 的空间复杂度。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        List<ListNode> list = new ArrayList<>();
        ListNode iter = head;
        while (iter != null) {
            list.add(iter);
            iter = iter.next;
        }
        
        TreeNode root = buildTree(list, 0, list.size() - 1);
        return root;
    }
    
    private TreeNode buildTree(List<ListNode> list, int start, int end) {
        if (start > end) return null;
        
        int mid = start + (end - start) / 2;
        TreeNode left = buildTree(list, start, mid - 1);
        TreeNode right = buildTree(list, mid + 1, end);
        TreeNode root = new TreeNode(list.get(mid).val);
        root.left = left;
        root.right = right;
        return root;
    }
}
{% endhighlight %}

# discuss

有序单链表实际上是一个 BST 的中序遍历，所以我们只需要按照中序遍历的顺序构造 BST 即可，因为要用到栈或者递归，空间复杂度降低到 O(logn)

{% highlight java %}
private ListNode node;

public TreeNode sortedListToBST(ListNode head) {
	if(head == null){
		return null;
	}
	
	int size = 0;
	ListNode runner = head;
	node = head;
	
	while(runner != null){
		runner = runner.next;
		size ++;
	}
	
	return inorderHelper(0, size - 1);
}

public TreeNode inorderHelper(int start, int end){
	if(start > end){
		return null;
	}
	
	int mid = start + (end - start) / 2;
	TreeNode left = inorderHelper(start, mid - 1);
	
	TreeNode treenode = new TreeNode(node.val);
	treenode.left = left;
	node = node.next;

	TreeNode right = inorderHelper(mid + 1, end);
	treenode.right = right;
	
	return treenode;
}
{% endhighlight %}