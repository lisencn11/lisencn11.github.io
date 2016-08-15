---
layout: post
title: Leetcode Problem 345 Summary
date: 2016-04-23
categories: blog
tags: [study]

---

# 题目

输入一个字符串，将字符串中的元音字母序列reverse。

如："hello"->"holle"


# 算法

很简单的算法，两个pointer从头尾同时出发扫描元音即可，但是有许多细节要注意：

* 元音判断，我用的方法略傻，挨个元音字符比较判断，更好的方法是用一个String vowel = "aoeiuAOEIU"记录元音，然后用vowel.contains(str)判断是否是元音
* 元音判断更好的方法是用HashSet，可以达到原来1/10的复杂度
* 刚开始被例子误导，没有考虑大写的元音
* 边界条件，如判断是否元音的同时还要判断pointer是否越界，因为存在情况：String中没有vowel

update: 二刷使用StringBuilder和HashSet

# 代码

{% highlight java %}
public class Solution {
    public String reverseVowels(String s) {
        if (s == null || s.length() < 2) {
            return s;
        }
        
        char[] str = s.toCharArray();
        int low = 0;
        int high = str.length - 1;
        char tmp = 0;
        while (low < high) {
            while (low < str.length 
                    && str[low] != 'a' && str[low] != 'o' && str[low] != 'i' && str[low] != 'e' && str[low] != 'u'
                    && str[low] != 'A' && str[low] != 'O' && str[low] != 'I' && str[low] != 'E' && str[low] != 'U') {
                low++;
            }
            while (high >= 0 
                    && str[high] != 'a' && str[high] != 'o' && str[high] != 'i' && str[high] != 'e' && str[high] != 'u'
                    && str[high] != 'A' && str[high] != 'O' && str[high] != 'I' && str[high] != 'E' && str[high] != 'U') {
                high--;
            }
            if (low < high) {
                tmp = str[low];
                str[low] = str[high];
                str[high] = tmp;
                low++;
                high--;
            }
        }
        return new String(str);
    }
}
{% endhighlight %}

### 二刷

{% highlight java %}
public class Solution {
    public String reverseVowels(String s) {
        Set<Character> vowels = new HashSet<>();
        vowels.add('A');
        vowels.add('a');
        vowels.add('I');
        vowels.add('i');
        vowels.add('O');
        vowels.add('o');
        vowels.add('U');
        vowels.add('u');
        vowels.add('E');
        vowels.add('e');
        
        StringBuilder res = new StringBuilder();
        int vowel = s.length() - 1;
        
        for (int i = 0; i < s.length(); i++) {
            if (!vowels.contains(s.charAt(i))) {
                res.append(s.charAt(i));
            } else {
                while (!vowels.contains(s.charAt(vowel--))){}
                res.append(s.charAt(vowel + 1));
            }
        }
        
        return res.toString();
    }
}
{% endhighlight %}