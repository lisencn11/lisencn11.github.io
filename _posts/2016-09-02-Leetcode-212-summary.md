---
layout: post
title: Leetcode Problem 212 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

For example,  
Given words = ["oath","pea","eat","rain"] and board =

[  
  ['o','a','a','n'],  
  ['e','t','a','e'],  
  ['i','h','k','r'],  
  ['i','f','l','v']  
]  
Return ["eat","oath"].

Note:  
You may assume that all inputs are consist of lowercase letters a-z.

You would need to optimize your backtracking to pass the larger test. Could you stop backtracking earlier?

If the current candidate does not exist in all words' prefix, you could stop backtracking immediately. What kind of data structure could answer such query efficiently? Does a hash table work? Why or why not? How about a Trie? If you would like to learn how to implement a basic trie, please work on this problem: Implement Trie (Prefix Tree) first.

# 我的算法

用 Trie 树解决，需要自己写 Trie 树

# 代码

{% highlight java %}
public class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Set<String> set = new HashSet<>();
        Trie trie = new Trie();
        for (String s : words) {
            trie.insert(s);
        }
        
        int m = board.length, n = board[0].length;
        StringBuilder sb = new StringBuilder();
        boolean[][] visited = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                visited[i][j] = false;
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dfs(board, trie, i, j, set, sb, visited);
            }
        }
        return new ArrayList<>(set);
    }
    
    private void dfs(char[][] board, Trie trie, int row, int col, Set<String> set, StringBuilder sb, boolean[][] visited) {
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) return;
        if (visited[row][col]) return;
        
        sb.append(board[row][col]);
        if (!trie.startsWith(sb.toString())) {
            sb.deleteCharAt(sb.length() - 1);
            return;
        }
        
        if (trie.search(sb.toString())) {
            set.add(sb.toString());
        }
        
        visited[row][col] = true;
        dfs(board, trie, row - 1, col, set, sb, visited);
        dfs(board, trie, row + 1, col, set, sb, visited);
        dfs(board, trie, row, col - 1, set, sb, visited);
        dfs(board, trie, row, col + 1, set, sb, visited);
        visited[row][col] = false;
        sb.deleteCharAt(sb.length() - 1);
    }
}

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

class Trie {
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