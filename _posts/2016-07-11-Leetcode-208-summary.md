---
layout: post
title: Leetcode Problem 208 Summary
date: 2016-07-11
categories: blog
tags: [study]

---

# 题目

实现Trie，字典树，前序树的insert，search，startWith方法。

# 我的算法

每个树节点中包含两个域，一个String域用于存储当前节点中的String，还有一个节点数组TrieNode域用于存储子节点。

# 代码

{% highlight java %}
class TrieNode {
    // Initialize your data structure here.
    public TrieNode[] children;
    public String word;
    
    public TrieNode() {
        children = new TrieNode[26];
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        if (word == null || word.length() == 0) return;
        
        char[] c = word.toCharArray();
        TrieNode node = root;
        for (int i = 0; i < c.length; i++) {
            int index = c[i] - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        node.word = word;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        if (word == null || word.length() == 0) return false;
        
        char[] c = word.toCharArray();
        TrieNode node = root;
        for (int i = 0; i < c.length; i++) {
            int index = c[i] - 'a';
            if (node.children[index] == null) {
                return false;
            } else {
                node = node.children[index];
            }
        }
        if (node.word != null && node.word.equals(word)) {
            return true;
        } else {
            return false;
        }
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        if (prefix == null || prefix.length() == 0) return false;
        
        char[] c = prefix.toCharArray();
        TrieNode node = root;
        for (int i = 0; i < c.length; i++) {
            int index = c[i] - 'a';
            if (node.children[index] == null) {
                return false;
            }
            node = node.children[index];
        }
        return true;
    }
}

// Your Trie object will be instantiated and called as such:
// Trie trie = new Trie();
// trie.insert("somestring");
// trie.search("key");
{% endhighlight %}

# 二刷

{% highlight java %}
class TrieNode {
    // Initialize your data structure here.
    public char item;
    public TrieNode[] children = new TrieNode[26];
    boolean end;
    public TrieNode() {
        
    }
    public TrieNode(char item) {
        this.item = item;
        end = false;
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        char[] words = word.toCharArray();
        TrieNode iter = root;
        for (int i = 0; i < words.length; i++) {
            int index = (int) (words[i] - 'a');
            if (iter.children[index] == null) {
                TrieNode child = new TrieNode(words[i]);
                iter.children[index] = child;
            }
            iter = iter.children[index];
        }
        iter.end = true;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        char[] words = word.toCharArray();
        TrieNode iter = root;
        for (int i = 0; i < words.length; i++) {
            int index = (int) (words[i] - 'a');
            if (iter.children[index] == null) return false;
            iter = iter.children[index];
        }
        return iter.end;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        char[] words = prefix.toCharArray();
        TrieNode iter = root;
        for (int i = 0; i < words.length; i++) {
            int index = (int) (words[i] - 'a');
            if (iter.children[index] == null) return false;
            iter = iter.children[index];
        }
        return true;
    }
}

// Your Trie object will be instantiated and called as such:
// Trie trie = new Trie();
// trie.insert("somestring");
// trie.search("key");
{% endhighlight %}