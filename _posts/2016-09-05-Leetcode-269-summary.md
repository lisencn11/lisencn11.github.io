---
layout: post
title: Leetcode Problem 269 Summary
date: 2016-09-05
categories: blog
tags: [study]

---

# 题目

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

For example,  
Given the following words in dictionary,

[  
  "wrt",  
  "wrf",  
  "er",  
  "ett",  
  "rftt"  
]  
The correct order is: "wertf".

Note:
1. You may assume all letters are in lowercase.
2. If the order is invalid, return an empty string.
3. There may be multiple valid order of letters, return any one of them is fine.

# 我的算法

先将先后顺序做成一个图，只需要对相邻的两个字符串进行比较，可以证明无需对不相邻的进行比较

再对图进行拓扑排序得到先后顺序

# 代码

{% highlight java %}
public class Solution {
    public String alienOrder(String[] words) {
        int len = words.length;
        Map<Character, Set<Character>> map = new HashMap<>();
        Set<Character> letters = new HashSet<>();

        for (String s : words) {
            for (int i = 0; i < s.length(); i++) {
                letters.add(s.charAt(i));
            }
        }
        for (int i = 0; i < len - 1; i++) {
            String first = words[i];
            String second = words[i + 1];
            int min = Math.min(first.length(), second.length());
            for (int j = 0; j < min; j++) {
                char firstC = first.charAt(j);
                char secondC = second.charAt(j);
                if (firstC == secondC) {
                    continue;
                } else {
                    if (!map.containsKey(firstC)) map.put(firstC, new HashSet<>());
                    map.get(firstC).add(secondC);
                    break;
                }
            }
        }
        
        int[] indegree = new int[26];
        Arrays.fill(indegree, 0);
        List<Character> list = new ArrayList<>(letters);
        for (Map.Entry<Character, Set<Character>> entry : map.entrySet()) {
            Set<Character> set = entry.getValue();
            for (Character c : set) {
                indegree[(int) (c - 'a')]++;
            }
        }
        
        Queue<Character> queue = new LinkedList<>();
        StringBuilder sb = new StringBuilder();
        for (Character c : list) {
            if (indegree[(int) (c - 'a')] == 0) {
                sb.append(c);
                queue.offer(c);
            }
        }

        while (!queue.isEmpty()) {
            char node = queue.poll();
            Set<Character> nb = map.get(node);
            if (nb != null) {
                for (Character c : nb) {
                    indegree[(int) (c - 'a')]--;
                    if (indegree[(int) (c - 'a')] == 0) {
                        sb.append(c);
                        queue.offer(c);
                    }
                }
            }
            map.remove(node);
        }
        if (map.size() != 0) return "";
        return sb.toString();
    }
}
{% endhighlight %}