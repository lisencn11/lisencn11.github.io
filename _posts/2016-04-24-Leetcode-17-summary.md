---
layout: post
title: Leetcode Problem 17 Summary
date: 2016-04-24
categories: blog
tags: [study]

---

# 题目

输入一串数字，每个数字按照手机九宫格键盘对应多个字母，要求输出这串数字所对应的所有可能的字母的集合。

如： "23" 对应 ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]


# 算法

非常直观的回溯算法，对于一个数字串"2345"，只要将"345"的结果得到，再将2对应的每个字母添加到都添加到已有的结果即可。

# 代码

{% highlight java %}
public class Solution {
    
    String[] map = new String[8];
    
    private List<String> recursiveGetCombination(String digits) {
        List<String> result = new ArrayList<String>();
        char cur = digits.charAt(0);
        int index = Character.getNumericValue(cur) - 2;
        
        if (digits.length() == 1) {
            for (int i = 0; i < map[index].length(); i++) {
                result.add(map[index].substring(i, i+1));
            }
            return result;
        }
        
        List<String> list = recursiveGetCombination(digits.substring(1));
        
        for (int i = 0; i < map[index].length(); i++) {
            for (int j = 0; j < list.size(); j++) {
                result.add(map[index].charAt(i) + list.get(j));
            }
        }
        
        return result;
    }
    
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return new ArrayList<String>();
        }
        
        map[0] = new String("abc");
        map[1] = new String("def");
        map[2] = new String("ghi");
        map[3] = new String("jkl");
        map[4] = new String("mno");
        map[5] = new String("pqrs");
        map[6] = new String("tuv");
        map[7] = new String("wxyz");
        
        List<String> result = recursiveGetCombination(digits);
        return result;
    }
}
{% endhighlight %}

# 更进一步

Discussion中得票最高的答案使用Queue实现了非递归的方法，对每个n-1长度数字串生成的字符串，在第n个数字时取出，在尾部加字母，在扔回队列，使用Queue这个数据结构十分巧妙。

{% highlight java %}
public List<String> letterCombinations(String digits) {

    LinkedList<String> ans = new LinkedList<String>();
    String[] mapping = new String[] {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    ans.add("");
    
    for(int i =0; i<digits.length();i++){
        int x = Character.getNumericValue(digits.charAt(i));
        
        while(ans.peek().length()==i){
            String t = ans.remove();
            for(char s : mapping[x].toCharArray())
                ans.add(t+s);
        }
    }
    return ans;
}
{% endhighlight %}

# 二刷

以前写的代码简直渣爆。。。

{% highlight java %}
public class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) return new ArrayList<>();
        String[] map = new String[]{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        List<String> list = new ArrayList<>();
        list.add("");
        char[] numbers = digits.toCharArray();
        
        for (int i = 0; i < numbers.length; i++) {
            List<String> temp = new ArrayList<>();
            char digit = numbers[i];
            String str = map[(int) (digit - '2')];
            for (String s : list) {
                for (int j = 0; j < str.length(); j++) {
                    temp.add(s + str.charAt(j));
                }
            }
            list = temp; 
        }
        
        return list;
    }
}
{% endhighlight %}