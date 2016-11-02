---
layout: post
title: Leetcode Problem 425 Summary
date: 2016-10-20
categories: blog
tags: [study]

---

# 题目

Given a set of words (without duplicates), find all word squares you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence ["ball","area","lead","lady"] forms a word square because each word reads the same both horizontally and vertically.

b a l l  
a r e a  
l e a d  
l a d y

Note:

1. There are at least 1 and at most 1000 words.
2. All words will have the exact same length.
3. Word length is at least 1 and at most 5.
4. Each word contains only lowercase English alphabet a-z.

Example 1:

Input:  
["area","lead","wall","lady","ball"]

Output:  
[  
  [ "wall",  
    "area",  
    "lead",  
    "lady"  
  ],  
  [ "ball",  
    "area",  
    "lead",  
    "lady"  
  ]  
]

Explanation:  
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

Example 2:

Input:  
["abat","baba","atan","atal"]

Output:  
[  
  [ "baba",  
    "abat",  
    "baba",  
    "atan"  
  ],  
  [ "baba",  
    "abat",  
    "baba",  
    "atal"  
  ]  
]

Explanation:  
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

# 我的算法

思路，首先思考，对于已知的i行，如何得到valid i+1行candidate。

第i+1行的candidate显然是有着i+1列前缀的所有字符串，问题变成：给定一个前缀，找到数组中含有该前缀的字符串。

前缀问题，自然想到用Trie树做。

# 代码

{% highlight java %}
public class Solution {
        
    public static final int LETTER_NUMBER = 26;
    
    class TrieNode {
        List<String> startWith;
        TrieNode[] children;
        
        TrieNode() {
            startWith = new ArrayList<>();
            children = new TrieNode[LETTER_NUMBER];
        }
    }
    
    class Trie {
        TrieNode root;
        
        Trie(String[] words) {
            root = new TrieNode();
            for (String word : words) {
                TrieNode currNode = root;
                for (int i = 0; i < word.length(); i++) {
                    int index = (int) (word.charAt(i) - 'a');
                    currNode.startWith.add(word);
                    if (currNode.children[index] == null) {
                        currNode.children[index] = new TrieNode();
                    }
                    currNode = currNode.children[index];
                }
            }
        }
        
        List<String> findByPrefix(String prefix) {
            TrieNode currNode = root;
            for (int i = 0; i < prefix.length(); i++) {
                int index = (int) (prefix.charAt(i) - 'a');
                if (currNode.children[index] == null) {
                    return new ArrayList<>();
                }
                currNode = currNode.children[index];
            }
            return currNode.startWith;
        }
    }
    
    public List<List<String>> wordSquares(String[] words) {
        List<List<String>> result = new ArrayList<>();
        
        if (words == null || words.length == 0) return result;
        
        Trie trie = new Trie(words);
        List<String> currList = new ArrayList<>();
        int len = words[0].length();
        
        for (String word : words) {
            currList.add(word);
            wordSquaresHelper(result, currList, trie, len);
            currList.remove(currList.size() - 1);
        }
        
        return result;
    }
    
    private void wordSquaresHelper(List<List<String>> result, List<String> currList, Trie trie, int len) {
        if (currList.size() == len) {
            result.add(new ArrayList<>(currList));
            return;
        }
        
        StringBuilder prefix = new StringBuilder();
        int col = currList.size();
        
        for (String word : currList) {
            prefix.append(word.charAt(col));
        }
        
        List<String> startWith = trie.findByPrefix(prefix.toString());
        for (String sw : startWith) {
            currList.add(sw);
            wordSquaresHelper(result, currList, trie, len);
            currList.remove(currList.size() - 1);
        }
    }
    
}
{% endhighlight %}