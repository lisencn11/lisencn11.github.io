---
layout: post
title: Leetcode Problem 211 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

设计一个数据结构，实现以下两个功能：

1. void addWord(word)
2. bool search(word)

search(word)包含 a - z 和 '.' 。'.' 代表任意一个字母。

# 我的算法

使用字典树数据结构，递归深度优先搜索查询word

# 代码

{% highlight java %}
public class WordDictionary {
    public class CharNode {
        public Map<Character, CharNode> next = null;
        boolean end;
        
        public CharNode() {
            next = new HashMap<>();
            end = false;
        }
    }
    CharNode root = null;
    
    public WordDictionary() {
        root = new CharNode();
    }

    // Adds a word into the data structure.
    public void addWord(String word) {
        if (word == null || word.length() == 0) return;
        char[] words = word.toCharArray();
        CharNode iter = root;
        
        for (int i = 0; i < words.length; i++) {
            if (!iter.next.containsKey(words[i])) iter.next.put(words[i], new CharNode());
            iter = iter.next.get(words[i]);
        }
        iter.end = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        if (word == null || word.length() == 0) return true;
        return searchHelper(word, root);

    }
    
    private boolean searchHelper(String word, CharNode iter) {
        if (word.length() == 0) {
            if (iter.end) return true;
            else return false;
        }
        
        char cur = word.charAt(0);
        if (cur == '.') {
            for (Map.Entry<Character, CharNode> entry : iter.next.entrySet()) {
                if (searchHelper(word.substring(1), entry.getValue())) return true;
            }
        } else {
            if (iter.next.containsKey(cur)) {
                if (searchHelper(word.substring(1), iter.next.get(cur))) return true;
            }
        }
        return false;
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
{% endhighlight %}

# 改进

开始使用HashMap存储子节点时有时候会超时，于是改用数组，超时解决。

{% highlight java %}
public class WordDictionary {
    public class CharNode {
        CharNode[] next = null;
        boolean end;
        
        public CharNode() {
            next = new CharNode[26];
            end = false;
        }
    }
    CharNode root = null;
    
    public WordDictionary() {
        root = new CharNode();
    }

    // Adds a word into the data structure.
    public void addWord(String word) {
        if (word == null || word.length() == 0) return;
        char[] words = word.toCharArray();
        CharNode iter = root;
        
        for (int i = 0; i < words.length; i++) {
            if (iter.next[words[i] - 'a'] == null) iter.next[words[i] - 'a'] = new CharNode();
            iter = iter.next[words[i] - 'a'];
        }
        iter.end = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        if (word == null || word.length() == 0) return true;
        return searchHelper(word, root);

    }
    
    private boolean searchHelper(String word, CharNode iter) {
        if (word.length() == 0) {
            if (iter.end) return true;
            else return false;
        }
        
        char cur = word.charAt(0);
        if (cur == '.') {
            for (int i = 0; i < iter.next.length; i++) {
                if (iter.next[i] != null && 
                    searchHelper(word.substring(1), iter.next[i])) return true;
            }
        } else {
            if (iter.next[cur - 'a'] != null) {
                if (searchHelper(word.substring(1), iter.next[cur - 'a'])) return true;
            }
        }
        return false;
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
{% endhighlight %}