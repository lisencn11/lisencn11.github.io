---
layout: post  
title: CC150 Chapter 9---Recursion and DP Questions  
date: 2016-06-29 
categories: blog  
tags: [study]

---

### n级梯子，一个小孩一次能上1级，2级或3级，一共有多少种方法上到第n级？

动态规划递推关系式：

a<sub>n</sub>为上到第n级的方法，则a<sub>n</sub> = a<sub>n-1</sub> + a<sub>n-2</sub> + a<sub>n-3</sub>。

{% highlight java %}
public class Solution0 {
    private static final int STEPS = 4;

    public int possibleWays(int steps) {
        if (steps < 1) {
            return 0;
        } else if (steps == 1) {
            return 1;
        } else if (steps == 2) {
            return 2;
        } else if (steps == 3) {
            return 4;
        }

        int[] ways = new int[steps];
        ways[0] = 1;
        ways[1] = 2;
        ways[2] = 4;
        for (int i = 3; i < steps; i++) {
            ways[i] = ways[i - 1] + ways[i - 2] + ways[i - 3];
        }

        return ways[steps - 1];
    }

    public static void main(String[] args) {
        Solution0 solution = new Solution0();
        System.out.println(solution.possibleWays(STEPS) + " ways to run up the stairs.");
    }
}
{% endhighlight %}

### 机器人从XY坐标轴的左上角向右下角移动，只能向右或下移每次，从(0,0)到(X,Y)有多少种方法？

动态规划递推关系式：

a<sub>i,j</sub>为走到坐标(i,j)的方法数，则a<sub>i,j</sub> = a<sub>i-1,j</sub> + a<sub>i,j-1</sub>。

{% highlight java %}
public class Solution1 {
    public static final int X = 3;
    public static final int Y = 3;

    public int possibleWays(int x, int y) {
        if (x < 0 || y < 0) {
            return -1;
        }
        int[][] grid = new int[x][y];
        for (int i = 0; i < x; i++) {
            grid[i][0] = 1;
        }
        for (int i = 0; i < y; i++) {
            grid[0][i] = 1;
        }

        for (int i = 1; i < x; i++) {
            for (int j = 1; j < y; j++) {
                grid[i][j] = grid[i - 1][j] + grid[i][j - 1];
            }
        }

        return grid[x - 1][y - 1];
    }

    public static void main(String[] args) {
        Solution1 solution = new Solution1();
        System.out.println(solution.possibleWays(X, Y) + " ways to get to (" + X + "," + Y + ")");
    }
}
{% endhighlight %}

### 定义magic数为A[i]=i，在一个有序不重复数组中查找magic数的index

有序数组查找，直接想到binary search。

{% highlight java %}
public class Solution2 {
    public static final int[] ARRAY = {-40, -20, -1, 1, 2, 3, 5, 7, 9, 12, 13};

    public int findMagic(int[] array) {
        if (array == null || array.length == 0) {
            return -1;
        }
        int low = 0;
        int high = array.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (array[mid] > mid) {
                high = mid - 1;
            } else if (array[mid] < mid) {
                low = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        Solution2 solution = new Solution2();
        System.out.println("index of magic number is " + solution.findMagic(ARRAY));
    }
}
{% endhighlight %}

##### FOLLOW UP

如果数组中有重复元素怎么办？

假设A[3] = 5，有重复元素的情况下，A[5] = 5也是有可能的。

但是我们会发现A[4] = 4是不可能的，这帮助我们优化算法。

{% highlight java %}
public class Solution2 {
    public static final int[] ARRAY = {-40, -20, -1, 1, 2, 3, 5, 7, 9, 12, 13};

    public int findMagicNotDistinct(int[] array, int start, int end) {
        if (end < start || start < 0 ||end >= array.length) {
            return -1;
        }
        int midIndex = start + (end - start) / 2;
        int midValue = array[midIndex];
        if (midValue == midIndex) {
            return midIndex;
        }

        int leftIndex = Math.min(midIndex - 1, midValue);
        int left = findMagicNotDistinct(array, start, leftIndex);
        if (left >= 0) {
            return left;
        }

        int rightIndex = Math.max(midIndex + 1, midValue);
        int right = findMagicNotDistinct(array, rightIndex, end);

        return right;
    }

    public static void main(String[] args) {
        Solution2 solution = new Solution2();
        System.out.println("index of magic number is " + solution.findMagicNotDistinct(ARRAY, 0, ARRAY.length - 1));
    }
}
{% endhighlight %}

### 写一个方法返回一个集合的所有子集

递归，先求n-1个元素的所有子集，再在其中加入第n个元素

{% highlight java %}
import java.util.ArrayList;
import java.util.List;

public class Solution3 {
    public List<List<Integer>> getSubsets(List<Integer> list, int index) {
        List<List<Integer>> allSubsets;
        if (list.size() == index) {
            allSubsets = new ArrayList<List<Integer>>();
            allSubsets.add(new ArrayList<Integer>());
        } else {
            allSubsets = getSubsets(list, index + 1);
            int item = list.get(index);
            List<List<Integer>> moreSubsets = new ArrayList<List<Integer>>();
            for (List<Integer> subset : allSubsets) {
                List<Integer> newSubset = new ArrayList<Integer>();
                newSubset.addAll(subset);
                newSubset.add(item);
                moreSubsets.add(newSubset);
            }
            allSubsets.addAll(moreSubsets);
        }
        return allSubsets;
    }

    public static void main(String[] args) {
        Solution3 solution = new Solution3();
        List<Integer> initialList = new ArrayList<Integer>();
        initialList.add(1);
        initialList.add(2);
        initialList.add(3);
        initialList.add(4);
        initialList.add(5);
        initialList.add(6);
        initialList.add(7);
        List<List<Integer>> listOfList = solution.getSubsets(initialList, 0);
        System.out.println("subset:");
        for (List<Integer> list : listOfList) {
            if (list.size() == 0) {
                System.out.print("NULL");
            }
            for (Integer i : list) {
                System.out.print(i + " ");
            }
            System.out.println();
        }
    }
}
{% endhighlight %}