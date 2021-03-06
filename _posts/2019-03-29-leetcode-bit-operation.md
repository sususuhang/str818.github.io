---
layout: article
title: Leetcode —「位运算」系列题解
tags: Leetcode

lang: zh-Hans
key: Leetcode_Bit_Operation
pageview: true
toc: true
show_subscribe: false
---

## 单一数字

[Leetcode - 136 Single Number (Easy)](https://leetcode.com/problems/single-number/)

题目描述：给定一个非空整数数组，数组中除了一个数字只出现一次外，其它的都出现两次，找到这个单一数字。

```
Input: [2,2,1]
Output: 1
```

解题思路：异或运算，同 0 异 1。

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int i : nums) {
        result ^= i;
    }
    return result;
}
```


## 汉明距离

[Leetcode - 461 Hamming Distance (Easy)](https://leetcode.com/problems/hamming-distance/)

题目描述：汉明距离是指两个数字二进制数对应位不同的个数，计算两个数字的汉明距离。

```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

解法一：

```java
public int hammingDistance(int x, int y) {
    int xor = x ^ y, count = 0;
    for (int i = 0; i < 32; i++) {
        count += (xor >> i) & 1; 
    }
    return count;
}
```

解法二：

```java
public int hammingDistance(int x, int y) {
    return Integer.bitCount(x ^ y);
}
```

## 二进制数中 1 的个数

[Leetcode - 191 Number of 1 Bits (Easy)](https://leetcode.com/problems/number-of-1-bits/)

解法一：判断每一位是否为 1，二进制数有多少位就需要遍历多少次。

```java
public int hammingWeight(int n) {
    int bits = 0;
    int mask = 1;
    for (int i = 0; i < 32; i++) {
        if ((n & mask) != 0) {
            bits++;
        }
        mask <<= 1;
    }
    return bits;
}
```

解法二：利用 x = x & (x - 1) 消除最低位的 1，直到消除到 0, 有多少 1 就需要遍历多少次。

```java
public int hammingWeight(int n) {
    int sum = 0;
    while (n != 0) {
        sum++;
        n &= (n - 1);
    }
    return sum;
}
```

## 二的次方

[Leetcode - 231 Power of Two (Easy)](https://leetcode.com/problems/power-of-two/)

题目描述：判断输入整数是否为 2 的次方。

解题思路：2 的次方二进制表示只会有一位是 1。

```java
public boolean isPowerOfTwo(int n) {
    return n > 0 && ((n & (n - 1)) == 0);
}
```

## 计算位数

[Leetcode - 338 Counting Bits (Medium)](https://leetcode.com/problems/counting-bits/)

题目描述：给定一个数 num，返回 0 <= i <= num 的所有二进制形式中包含的 1 的个数。

解题思路：在去掉最低位后 1 的个数的基础上加 1。

```java
public int[] countBits(int num) {
    int[] bits = new int[num + 1];
    for(int i = 1; i <= num; i++){
        bits[i] = bits[i & (i - 1)] + 1; 
    }
    return bits;
}
```

## N 皇后 II

[Leetcode - 52 N-Queens II (Hard)](https://leetcode.com/problems/n-queens-ii/)

```java
int count = 0;
public int totalNQueens(int n) {
    dfs(n, 0, 0, 0, 0);
    return count;
}

public void dfs(int n, int row, int cols, int pie, int na){
    if(row >= n){
        count++;
        return;
    }
    // 得到当前所有的空位
    int bits = (~(cols | pie | na)) & ((1 << n) - 1);
    
    while(bits != 0){
        int p = bits & -bits;   // 取到最低位的 1
        dfs(n, row + 1, cols | p, (pie | p) << 1, (na | p) >>1);
        bits = bits & (bits - 1); // 去掉最低位的 1
    }
}
```