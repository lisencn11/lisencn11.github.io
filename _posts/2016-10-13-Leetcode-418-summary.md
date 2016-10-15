---
layout: post
title: Leetcode Problem 418 Summary
date: 2016-10-13
categories: blog
tags: [study]

---

# 题目

Given a rows x cols screen and a sentence represented by a list of words, find how many times the given sentence can be fitted on the screen.

Note:

1. A word cannot be split into two lines.
2. The order of words in the sentence must remain unchanged.
3. Two consecutive words in a line must be separated by a single space.
4. Total words in the sentence won't exceed 100.
5. Length of each word won't exceed 10.
6. 1 ≤ rows, cols ≤ 20,000.

Example 1:

Input:  
rows = 2, cols = 8, sentence = ["hello", "world"]

Output:   
1

Explanation:  
hello---  
world---

The character '-' signifies an empty space on the screen.

Example 2:

Input:  
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output:   
2

Explanation:  
a-bcd-   
e-a---  
bcd-e-

The character '-' signifies an empty space on the screen.

Example 3:

Input:  
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output:   
1

Explanation:  
I-had  
apple  
pie-I  
had--

The character '-' signifies an empty space on the screen.

# 我的算法

用两个数组：nextIndex[] 和 times[]，nextIndex[i]表示对以sentence数组中第i个String为开头的行，它的下一行以哪个String开头个；times[i]表示对以sentence数组中第i个String为开头的行，此行中到达过多少次sentence结尾。

利用这两个数组即可对每行快速定位下一行和本行包含的完整sentence数。

# 代码

{% highlight java %}
public class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        if (sentence == null || sentence.length == 0 || rows == 0 || cols == 0) return 0;
        
        int len = sentence.length;
        int[] nextIndex = new int[len];
        int[] times = new int[len];
        int result = 0;
        int currIndex = 0;
        
        for (int i = 0; i < sentence.length; i++) {
            int remain = cols;
            int index = i;
            int count = 0;
            
            while (remain >= sentence[index].length()) {
                remain -= sentence[index].length() + 1;
                index++;
                if (index == len) {
                    index = 0;
                    count++;
                }
            }
            
            times[i] = count;
            nextIndex[i] = index;
        }
        
        for (int i = 0; i < rows; i++) {
            result += times[currIndex];
            currIndex = nextIndex[currIndex];
        }
        
        return result;
    }
}
{% endhighlight %}