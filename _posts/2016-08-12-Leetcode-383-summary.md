---
layout: post
title: Leetcode Problem 383 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

 Given  an  arbitrary  ransom  note  string  and  another  string  containing  letters from  all  the  magazines,  write  a  function  that  will  return  true  if  the  ransom   note  can  be  constructed  from  the  magazines ;  otherwise,  it  will  return  false.   

Each  letter  in  the  magazine  string  can  only  be  used  once  in  your  ransom  note.

Note:
You may assume that both strings contain only lowercase letters.

canConstruct("a", "b") -> false  
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true  

# 我的算法

记录下magazine中各character出现的次数，check是否足够ransom note。

# 代码

{% highlight java %}
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        Map<Character, Integer> map = new HashMap<>();
        
        for (int i = 0; i < magazine.length(); i++) {
            if (!map.containsKey(magazine.charAt(i))) map.put(magazine.charAt(i), 0);
            map.put(magazine.charAt(i), map.get(magazine.charAt(i)) + 1);
        }
        
        for (int i = 0; i < ransomNote.length(); i++) {
            if (!map.containsKey(ransomNote.charAt(i))) return false;
            int temp = map.get(ransomNote.charAt(i)) - 1;
            if (temp < 0) return false;
            map.put(ransomNote.charAt(i), temp);
        }
        return true;
    }
}
{% endhighlight %}