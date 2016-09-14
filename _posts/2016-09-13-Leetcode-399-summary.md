---
layout: post
title: Leetcode Problem 135 Summary
date: 2016-09-13
categories: blog
tags: [study]

---

# 题目

Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:  
Given a / b = 2.0, b / c = 3.0.   
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .   
return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:

equations = [ ["a", "b"], ["b", "c"] ],  
values = [2.0, 3.0],  
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 

The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

# 我的算法

Union-Find 算法。相互有联系的变量会被转化成相同base的数。

# 代码

{% highlight java %}
public class Solution {
    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        if (queries == null || queries.length == 0) return new double[0];
        
        double[] result = new double[queries.length];
        Map<String, Node> map = new HashMap<>();
        for (int i = 0; i < equations.length; i++) {
            String s1 = equations[i][0], s2 = equations[i][1];
            if (!map.containsKey(s1) && !map.containsKey(s2)) {
                Node n1 = new Node();
                Node n2 = new Node();
                n1.value = values[i];
                n2.value = 1;
                n1.parent = n2;
                map.put(s1, n1);
                map.put(s2, n2);
            } else if (!map.containsKey(s1)) {
                Node n1 = new Node();
                Node n2 = map.get(s2);
                n1.value = values[i] * n2.value;
                n1.parent = n2;
                map.put(s1, n1);
            } else if (!map.containsKey(s2)) {
                Node n2 = new Node();
                Node n1 = map.get(s1);
                n2.value = n1.value / values[i];
                n2.parent = n1;
                map.put(s2, n2);
            } else {
                unionNode(map.get(s1), map.get(s2), values[i], map);
            }
        }
        
        int index = 0;
        for (String[] query : queries) {
            if (!map.containsKey(query[0]) || !map.containsKey(query[1]) || findParent(map.get(query[0])) != findParent(map.get(query[1]))) {
                result[index] = -1;
            } else {
                result[index] = map.get(query[0]).value / map.get(query[1]).value;
            }
            index++;
        }
        return result;
    }
    
    class Node {
        Node parent = this;
        double value = 0;
    }
    
    private void unionNode(Node n1, Node n2, double value, Map<String, Node> map) {
        Node p1 = findParent(n1), p2 = findParent(n2);
        double ratio = (n2.value * value) / n1.value;
        for (Map.Entry<String, Node> entry : map.entrySet()) {
            Node node = entry.getValue();
            if (findParent(node) == p1) {
                node.value *= ratio;
            }
        }
        p1.parent = p2;
    }
    
    private Node findParent(Node n) {
        if (n == n.parent) {
            return n;
        }
        n.parent = findParent(n.parent);
        return n.parent;
    }
}
{% endhighlight %}