---
layout: post
title: Leetcode Problem 130 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

**输入**一个 char 型二维数组，由 'X' 和 'O' 组成，实现算法将被 'X' 包围的 'O' （即整片 'O' 没有一个挨着边界）全部变成 'X'。

# 我的算法

使用广度优先搜索算法，对于遇到的 'O' 看其上下左右是否还有 'O' ，广度优先遍历所有从第一个 'O' 能到达的 'O' ，若遍历完成没有遇到边界节点则全部置为 'X' ，否则不变，对于遍历过的 'O' ，用另一个二维数组记忆以防止重复遍历。

# 代码

{% highlight java %}
import java.awt.Point;
public class Solution {
    boolean[][] visited;
    
    private void bfs(int i, int j, char[][] board) {
        Queue<Point> queue = new LinkedList<>();
        List<Point> list = new ArrayList<>();
        boolean captured = true;
        Point p = new Point(i, j);
        visited[i][j] = true;
        queue.offer(p);
        
        while (!queue.isEmpty()) {
            Point iter = queue.poll();
            int x = (int)iter.getX();
            int y = (int)iter.getY();
            if (x == 0 || y == 0 || x == visited.length - 1 || y == visited[0].length - 1)
                captured = false;
            list.add(iter);
            if (x - 1 >= 0 && !visited[x - 1][y] && board[x - 1][y] == 'O') {
                queue.offer(new Point(x - 1, y));
                visited[x - 1][y] = true;
            }
            if (x + 1 < board.length && !visited[x + 1][y] && board[x + 1][y] == 'O') {
                queue.offer(new Point(x + 1, y));
                visited[x + 1][y] = true;
            }
            if (y - 1 >= 0 && !visited[x][y - 1] && board[x][y - 1] == 'O') {
                queue.offer(new Point(x, y - 1));
                visited[x][y - 1] = true;
            }
            if (y + 1 < board[0].length && !visited[x][y + 1] && board[x][y + 1] == 'O') {
                queue.offer(new Point(x, y + 1));
                visited[x][y + 1] = true;
            }
        }
        
        if (captured) {
            for (Point iter : list) {
                int x = (int)iter.getX();
                int y = (int)iter.getY();
                board[x][y] = 'X';
            }
        }
    }
    
    public void solve(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) return;
        
        visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < visited.length; i++) {
            for (int j = 0; j < visited[0].length; j++) {
                visited[i][j] = false;
            }
        }
        
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 'O' && !visited[i][j]) {
                    bfs(i, j, board);
                }
            }
        }
                
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0) return;
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O') {
                bfs(board, i, 0);
            }
            if (board[i][n - 1] == 'O') {
                bfs(board, i, n - 1);
            }
        }
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O') {
                bfs(board, 0, j);
            }
            if (board[m - 1][j] == 'O') {
                bfs(board, m - 1, j);
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') board[i][j] = 'X';
                else if (board[i][j] == '1') board[i][j] = 'O';
            }
        }
    }
    
    private void bfs(char[][] board, int i, int j) {
        int[] shift = new int[]{0, 1, 0, -1, 0};
        board[i][j] = '1';
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{i, j});
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            for (int k = 1; k < shift.length; k++) {
                int x = point[0] + shift[k - 1];
                int y = point[1] + shift[k];
                if (x >= 0 && y >= 0 && x < board.length && y < board[0].length && board[x][y] == 'O') {
                    board[x][y] = '1';
                    queue.offer(new int[]{x, y});
                }
            }
        }
    }
}
{% endhighlight %}