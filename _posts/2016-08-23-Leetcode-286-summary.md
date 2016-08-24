---
layout: post
title: Leetcode Problem 286 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

You are given a m x n 2D grid initialized with these three possible values.

1. -1 - A wall or an obstacle.
2. 0 - A gate.
3. INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

For example, given the 2D grid:  
INF  -1  0  INF  
INF INF INF  -1  
INF  -1 INF  -1  
  0  -1 INF INF  

After running your function, the 2D grid should be:  
  3  -1   0   1  
  2   2   1  -1  
  1  -1   2  -1  
  0  -1   3   4  

# 我的算法

BFS 算法。

# 代码

{% highlight java %}
public class Solution {
    static final int INF = 2147483647;
    public void wallsAndGates(int[][] rooms) {
        for (int i = 0; i < rooms.length; i++) {
            for (int j = 0; j < rooms[0].length; j++) {
                if (rooms[i][j] != 0) continue;
                Pair pair = new Pair(i, j);
                Queue<Pair> queue = new LinkedList<>();
                queue.offer(pair);
                int level = 0;
                while (!queue.isEmpty()) {
                    int len = queue.size();
                    for (int k = 0; k < len; k++) {
                        pair = queue.poll();
                        int x = pair.x;
                        int y = pair.y;
                        rooms[x][y] = level < rooms[x][y] ? level : rooms[x][y];
                        if (x - 1 >= 0 && rooms[x - 1][y] > level + 1) queue.offer(new Pair(x - 1, y));
                        if (x + 1 < rooms.length && rooms[x + 1][y] > level + 1) queue.offer(new Pair(x + 1, y));
                        if (y - 1 >= 0 && rooms[x][y - 1] > level + 1) queue.offer(new Pair(x, y - 1));
                        if (y + 1 < rooms[0].length && rooms[x][y + 1] > level + 1) queue.offer(new Pair(x, y + 1));
                        
                    }
                    level++;
                }
            }
        }
    }
    
    class Pair {
        int x;
        int y;
        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
{% endhighlight %}

# discuss

事实上我应该把所有的 gate 都放入 Queue 之后再 BFS ，就像下面这样。

{% highlight java %}
public class Solution {
    public void wallsAndGates(int[][] rooms) {
        if (rooms.length == 0 || rooms[0].length == 0) return;
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < rooms.length; i++) {
            for (int j = 0; j < rooms[0].length; j++) {
                if (rooms[i][j] == 0) queue.add(new int[]{i, j});
            }
        }
        while (!queue.isEmpty()) {
            int[] top = queue.remove();
            int row = top[0], col = top[1];
            if (row > 0 && rooms[row - 1][col] == Integer.MAX_VALUE) {
                rooms[row - 1][col] = rooms[row][col] + 1;
                queue.add(new int[]{row - 1, col});
            }
            if (row < rooms.length - 1 && rooms[row + 1][col] == Integer.MAX_VALUE) {
                rooms[row + 1][col] = rooms[row][col] + 1;
                queue.add(new int[]{row + 1, col});
            }
            if (col > 0 && rooms[row][col - 1] == Integer.MAX_VALUE) {
                rooms[row][col - 1] = rooms[row][col] + 1;
                queue.add(new int[]{row, col - 1});
            }
            if (col < rooms[0].length - 1 && rooms[row][col + 1] == Integer.MAX_VALUE) {
                rooms[row][col + 1] = rooms[row][col] + 1;
                queue.add(new int[]{row, col + 1});
            }
        }
    }
}
{% endhighlight %}