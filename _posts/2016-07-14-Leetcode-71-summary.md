---
layout: post
title: Leetcode Problem 71 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

**输入**一个Unix风格的绝对路径。

**输出**简化后的绝对路径。

如：  
>path = "/home/", => "/home"  
path = "/a/./b/../../c/", => "/c"

# 我的算法

"/" 之间有下面几种可能情况：

1. "." 表示当前路径，可以忽略
2. ".." 表示上一路径，出栈（实际编码中忘记考虑根目录下应该忽略） 
3. "//" 无意义，忽略
4. "abcd..." 特定文件夹，入栈

另外边界条件：如果就是在根目录下，那么栈为空，输出 "/"

# 代码

{% highlight java %}
public class Solution {
    public String simplifyPath(String path) {
        if (path == null || path.length() == 0) return path;
        
        Stack<String> stack = new Stack<>();
        String[] folders = path.split("/");
        for (String folder : folders) {
            if (folder.length() == 0) continue;
            if (folder.equals("..") && !stack.isEmpty()) stack.pop();
            else if (folder.equals(".")) continue;
            else if (folder.equals("..") && stack.isEmpty()) continue;
            else stack.push(folder);
        }
        
        if (stack.isEmpty()) {
            return "/";
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.insert(0, stack.pop());
            sb.insert(0, "/");
        }
        
        return sb.toString();
    }
}
{% endhighlight %}