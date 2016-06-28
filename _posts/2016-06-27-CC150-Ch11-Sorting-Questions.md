---
layout: post  
title: CC150 Chapter 11---Sorting Questions  
date: 2016-06-27 
categories: blog  
tags: [study]

---

### 两个有序数组A和B，A的尾部有足够空间放B，写一个方法mergeB到A中成有序数组

**in place merge**，如果从头merge会导致A中元素被覆盖，但是从A的尾部到头部开始merge则可以避免覆盖。

```java
public class Solution0 {
    public void merge(int[] a, int[] b, int lastA, int lastB) {
        if (a == null || a.length == 0) {
            a = b;
            return;
        }
        if (b == null || b.length == 0) {
            return;
        }

        int last = lastA + lastB + 1;
        while ((last >= 0) && (lastA >= 0) && (lastB >= 0)) {
            if (a[lastA] >= b[lastB]) {
                a[last--] = a[lastA--];
            } else {
                a[last--] = b[lastB--];
            }
        }

        while (lastB >= 0) {
            a[last--] = a[lastB--];
        }
    }

    public static void main(String[] args) {
        int[] a = {1, 3, 5, 7, 9, -1, -1, -1, -1};
        int[] b = {2, 4, 6, 8};
        Solution0 solution = new Solution0();
        solution.merge(a, b, 4, 3);
        for (int i = 0; i <= (4 + 3 + 1); i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println();
    }
}
```

### 对一个字符串数组排序，另所有的anagram(所含字母相同但是顺序不同)互相挨着

##### 实现Comparator接口后，用Arrays.sort()排序

```java
import java.util.Arrays;
import java.util.Comparator;

public class AnagramComparator implements Comparator<String> {
    public int compare(String s1, String s2) {
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();
        Arrays.sort(c1);
        Arrays.sort(c2);
        return c1.toString().compareTo(c2.toString());
    }

    public static void main(String[] args) {
        String[] str = {"abcde", "bcdea", "abcdd"};
        Arrays.sort(str, new AnagramComparator());
        for (String s : str) {
            System.out.println(s);
        }
    }
}
```

##### 使用HashMap<String, List<String>>，以anagram排序后作为key，排序后相同的放入一个list

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;

public class Solution1 {

    public void sort(String[] strings) {
        List<String> list = null;
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        int index = 0;

        for (String s : strings) {
            char[] c = s.toCharArray();
            Arrays.sort(c);
            String key = String.valueOf(c);
            if (!map.containsKey(key)) {
                list = new LinkedList<String>();
            } else {
                list = map.get(key);
            }
            list.add(s);
            map.put(key, list);
        }

        for (String key : map.keySet()) {
            list = map.get(key);
            for (String s : list) {
                strings[index] = s;
                index++;
            }
        }
    }

    public static void main(String[] args) {
        String[] strings = {"abcd", "senli", "17463", "bdca", "47316", "lisen"};
        Solution1 solution = new Solution1();
        solution.sort(strings);

        for (String s : strings) {
            System.out.println(s);
        }
    }
}
```

### 一个排好序的整型数组，旋转(rotate)未知次，设计算法找到特定元素，假设数组升序排列

分类讨论即可，首先判断left-mid，mid-right这两段哪段为单调递增，然后根据target与array[mid]的比较结果判断target在mid的哪一边。

**注意**处理left，mid，和right相等的情况，如果只有left和mid相等，则可判断target在mid-right中；如果三个都相等，则无法判断left-mid段为定值，还是mid-right段为定值，故两端都要求解。

```java
public class Solution2 {
    public int find(int[] array, int target) {
        return search(array, 0, array.length - 1, target);
    }

    public int search(int[] array, int left, int right, int target) {
        int mid = left + (right - left) / 2;
        if (target == array[mid]) {
            return mid;
        }
        if (right < left) {
            return -1;
        }

        if (array[left] < array[mid]) {
            if (target >= array[left] && target <= array[mid]) {
                return search(array, left, mid - 1, target);
            } else {
                return search(array, mid + 1, right, target);
            }
        } else if (array[mid] < array[left]) {
            if (target >= array[mid] && target <=array[right]) {
                return search(array, mid + 1, right, target);
            } else {
                return search(array, left, mid - 1, target);
            }
        } else if (array[left] == array[left]) {
            if (array[mid] != array[right]) {
                return search(array, mid + 1, right, target);
            } else {
                int result = search(array, left, mid - 1, target);
                if (result == -1) {
                    return search(array, mid + 1, right, target);
                } else {
                    return result;
                }
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] array = {10, 15, 20, 0, 5};
        Solution2 solution = new Solution2();
        int index = solution.find(array, 20);
        System.out.println("index of element: " + index);
    }
}
```

### 假设有一个20GB的文件，每行是一个String。如何对文件进行排序

20GB暗示我们文件不能全部加载进内存，我们只能把部分文件加载进内存。

我们可以将文件分成若干个小部分，每部分**x**MB大小。对每个**x**MB大小的部分分别排序后，对于这些部分进行归并排序，这里我们只需要把每个部分的第一个元素载入内存即可，而不需要全部载入。

这个算法叫做**外部排序**。

### 一个有序String数组中散布着空String，写一个方法找到指定String的位置

二分查找，当mid为空时同时向左右查找最近的不为空的String，然后用compareTo()对两个String进行比较。

```java
public class Solution3 {
    public int searchR(String[] strings, String str) {
        if (strings == null || str == null || str.isEmpty()) {
            return -1;
        }
        return searchR(strings, str, 0, strings.length - 1);
    }

    public int searchR(String[] strings, String str, int low, int high) {
        if (low > high) {
            return -1;
        }
        int mid = low + (high - low) / 2;

        if (strings[mid].isEmpty()) {
            int left = mid - 1;
            int right = mid + 1;
            while (true) {
                if (left < low && right > high) {
                    return -1;
                } else if (right <= high && !strings[right].isEmpty()) {
                    mid = right;
                    break;
                } else if (left >= low && !strings[left].isEmpty()) {
                    mid = left;
                    break;
                }
                left--;
                right++;
            }
        }

        if (strings[mid].equals(str)) {
            return mid;
        } else if (strings[mid].compareTo(str) < 0) {
            return searchR(strings, str, mid + 1, high);
        } else {
            return searchR(strings, str, low, mid - 1);
        }
    }

    public static void main(String[] args) {
        String[] strings = {"hongmao", "lisen", "", "senli", "", "", "", "", "wenhong"};
        String target = "lisen";
        Solution3 solution = new Solution3();
        int index = solution.searchR(strings, target);
        System.out.println(index);
    }
}
```

### 一个M*N的矩阵，每一行和每一列都是升序排列，写一个方法找到指定元素

首先，我们可以对每一行使用二分查找，这样的时间复杂度是O(M*logN)。

然后，我们注意到，这个矩阵有以下特性：

* 如果一列的开头比target大，则这一列可以忽略。
* 如果一行的开头比target大，则这一行可以忽略。
* 如果一列的结尾比target小，则这一列可以忽略。
* 如果一行的结尾比target小，则这一行可以忽略。

我们将这四条特性相结合，我们希望无论当前元素比target大或小我们都能有所操作，所以结合一四或者二三。

```java
import java.awt.Point;

public class Solution4 {
    public Point find(int[][] matrix, int target) {
        int row = 0;
        int col = matrix[0].length - 1;
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                return new Point(row, col);
            } else if (matrix[row][col] > target) {
                col--;
            } else {
                row++;
            }
        }
        return new Point(-1, -1);
    }

    public static void main(String[] args) {
        int[][] matrix = {{15, 20, 40, 85}, {20, 35, 80, 95}, {30, 55, 95, 105}, {40, 80, 100, 120}};
        Solution4 solution = new Solution4();
        int target = 55;
        Point point = solution.find(matrix, target);
        System.out.println(point);
    }
}
```

*第二种算法较复杂，待日后充分理解再补充...*

### 马戏团表演叠罗汉，要求上面的人身高和体重同时要比下面的人小，写一个方法计算最多可能人数

思路是抽象为最长递增子序列问题。我们将人先按照身高进行排序，以排序后的顺序作为体重数组的下标就会发现，问题抽象为在按照身高排序后的体重数组中找到最长的递增子序列。

我们使用的是动态规划O(n<sup>2</sup>)复杂度的算法。从前到后依次计算以当前元素结尾的最长递增子序列是什么，这样需要遍历整个序列，为O(n)。对于每个元素的最长子序列计算，我们需要遍历其之前的所有元素，找到体重小于其的最长子序列，为O(n)。故最终为O(n<sup>2</sup>)。

下面代码中，我们写了一个Person类，实现了Comparable接口，override compareTo方法，以实现按照身高的排序。

```java
public class Person implements Comparable<Person> {
    private int height;
    private int weight;

    public Person(int height, int weight) {
        this.height = height;
        this.weight = weight;
    }

    public int getH() {
        return height;
    }

    public int getW() {
        return weight;
    }

    @Override
    public int compareTo(Person person) {
        if (this.height != person.getH()) {
            return ((Integer)this.height).compareTo(person.getH());
        } else {
            return ((Integer)this.weight).compareTo(person.getW());
        }
    }

    public boolean isBefore(Person p) {
        if (this.height < p.getH() && this.weight < p.getW()) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public String toString() {
        return "Height: " + height + " Weight: " + weight;
    }
}


import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution5 {
    public List<Person> getIncreasingSequence(List<Person> persons) {
        Collections.sort(persons);
        return longestIncreasingSubsequence(persons);
    }

    public List<Person> longestIncreasingSubsequence(List<Person> persons) {
        List<Person>[] solutions = new ArrayList[persons.size()];
        longestIncreasingSubsequence(persons, solutions, 0);

        List<Person> bestSequence = null;
        for (int i = 0; i < persons.size(); i++) {
            bestSequence = seqWithMaxLength(bestSequence, solutions[i]);
        }

        return bestSequence;
    }

    public void longestIncreasingSubsequence(List<Person> persons, List<Person>[] solutions, int currentIndex) {
        if (currentIndex >= persons.size() || currentIndex < 0) {
            return;
        }
        Person currentElement = persons.get(currentIndex);

        List<Person> bestSequence = null;
        for (int i = 0; i < currentIndex; i++) {
            if (persons.get(i).isBefore(currentElement)) {
                bestSequence = seqWithMaxLength(bestSequence, solutions[i]);
            }
        }

        List<Person> newSolution = new ArrayList<Person>();
        if (bestSequence != null) {
            newSolution.addAll(bestSequence);
        }
        newSolution.add(currentElement);

        solutions[currentIndex] = newSolution;
        longestIncreasingSubsequence(persons, solutions, currentIndex + 1);
    }

    public List<Person> seqWithMaxLength(List<Person> seq1, List<Person> seq2) {
        if (seq1 == null) return seq2;
        if (seq2 == null) return seq1;
        return seq1.size() > seq2.size() ? seq1 : seq2;
    }

    public static void main(String[] args) {
        List<Person> persons = new ArrayList<Person>();
        persons.add(new Person(0, 13));
        persons.add(new Person(1, 14));
        persons.add(new Person(2, 10));
        persons.add(new Person(3, 11));
        persons.add(new Person(4, 12));
        Solution5 solution = new Solution5();
        List<Person> list = solution.getIncreasingSequence(persons);
        for (Person p : list) {
            System.out.println(p);
        }
    }
}
```

### 不断的读一个整型流，写一个数据结构和算法，实现对于整型排名的追踪，即输入一个整型，返回小于等于这个整型的个数：对每个数调用，track(int x)；得到一个整型排名，getRankOfNumber(int x)

考虑使用二分查找树(binary search tree)实现，这样插入和查询排名的复杂度均是O(logn)。

每个二叉树节点有两个域，data域和leftRank域。data域是数据，leftRank是左子树中节点的数量。

插入数据的操作很简单，若数据小于等于当前数据则新建左子树或者交给左子树负责；大于当前数据同理。

查询数据分为小于，等于，大于三种情况。小于的话则交给左子树查询，返回左子树的结果；等于的话直接返回当前leftRank，即左子树节点个数即可；大于的话交给右子树，查询出的结果是在右子树中的rank，所以我们要**+leftRank+1**，即加上左子树和当前节点。

```java
public class RankNode {
    public int leftSize = 0;
    public RankNode left, right;
    public int data = 0;

    public RankNode(int data) {
        this.data = data;
    }

    public void insert(int newData) {
        if (newData <= data) {
            if (left != null) {
                left.insert(newData);
            } else {
                left = new RankNode(newData);
            }
            leftSize++;
        } else {
            if (right != null) {
                right.insert(newData);
            } else {
                right = new RankNode(newData);
            }
        }
    }

    public int getRank(int searchData) {
        if (searchData < data) {
            if (left != null) {
                return left.getRank(searchData);
            } else {
                return -1;
            }
        } else if (searchData > data) {
            int rightRank = 0;
            if (right != null) {
                rightRank = right.getRank(searchData);
            } else {
                return -1;
            }
            return leftSize + 1 + rightRank;
        } else {
            return leftSize;
        }
    }
}


public class Solution6 {
    private static RankNode root = null;

    public static void track(int number) {
        if (root == null) {
            root = new RankNode(number);
        } else {
            root.insert(number);
        }
    }

    public static int getRankOfNumber(int number) {
        return root.getRank(number);
    }

    public static void main(String[] args) {
        Solution6.track(0);
        Solution6.track(2);
        Solution6.track(4);
        Solution6.track(6);
        Solution6.track(8);
        Solution6.track(10);
        Solution6.track(1);
        Solution6.track(3);
        Solution6.track(5);
        Solution6.track(7);
        Solution6.track(9);
        Solution6.track(11);
        System.out.println(Solution6.getRankOfNumber(7));
    }
}
```