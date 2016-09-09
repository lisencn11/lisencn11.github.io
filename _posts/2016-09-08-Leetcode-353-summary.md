---
layout: post
title: Leetcode Problem 353 Summary
date: 2016-09-08
categories: blog
tags: [study]

---

# 题目

Design a Snake game that is played on a device with screen size = width x height. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

Example:  
Given width = 3, height = 2, and food = [[1,2],[0,1]].

Snake snake = new Snake(width, height, food);

Initially the snake appears at position (0,0) and the food at (1,2).

|S| | |  
| | |F|

snake.move("R"); -> Returns 0

| |S| |  
| | |F|

snake.move("D"); -> Returns 0

| | | |  
| |S|F|

snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )

| |F| |  
| |S|S|

snake.move("U"); -> Returns 1

| |F|S|  
| | |S|

snake.move("L"); -> Returns 2 (Snake eats the second food)

| |S|S|  
| | |S|

snake.move("U"); -> Returns -1 (Game over because snake collides with border)

# 我的算法

考察数据结构，用 HashSet 存储当前蛇的身体，用 ArrayDeque 当前蛇的身体，通过 HashSet 实现快速判断是否相撞，通过双向链表实现蛇的移动，食物地点存在一个队列 Queue 中。

# 代码

{% highlight java %}
public class SnakeGame {
    Set<String> occupy;
    ArrayDeque<String> snake;
    int width = 0;
    int height = 0;
    Queue<String> foods;

    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        occupy = new HashSet<>();
        snake = new ArrayDeque<>();
        foods = new LinkedList<>();
        String position = new String("0,0");
        occupy.add(position);
        snake.add(position);
        for (int[] f : food) {
            int x = f[0];
            int y = f[1];
            foods.offer(x + "," + y);
        }
    }
    
    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        String[] currPos = snake.getLast().split(",");
        int x = Integer.parseInt(currPos[0]);
        int y = Integer.parseInt(currPos[1]);
        int nextX = x;
        int nextY = y;
        if (direction.equals("U")) nextX--;
        if (direction.equals("L")) nextY--;
        if (direction.equals("R")) nextY++;
        if (direction.equals("D")) nextX++;
        
        String pos = new String(nextX + "," + nextY);
        if (nextX < 0 || nextX >= height || nextY < 0 || nextY >= width) return -1;
        if (occupy.contains(pos) && !pos.equals(snake.getFirst())) return -1;
        if (!foods.isEmpty()) {
            String[] currFood = foods.peek().split(",");
            int currFoodX = Integer.parseInt(currFood[0]);
            int currFoodY = Integer.parseInt(currFood[1]);
            if (nextX == currFoodX && nextY == currFoodY) {
                occupy.add(pos);
                snake.add(pos);
                foods.poll();
                return snake.size() - 1;
            }
        }

        String tail = snake.getFirst();
        occupy.remove(tail);
        snake.removeFirst();
        occupy.add(pos);
        snake.add(pos);
        return snake.size() - 1;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
 {% endhighlight %}