---
title: '[Algorithm]轮转数组'
catalog: true
date: 2020-11-01
subtitle: Rotate Array
lang: cn
header-img: 1.png
tags:
- Java
- Data Structures
- Algorithms
- Array
categories:
- Data Structures and Algorithms
---

---
### 题目
今天刷题的时候遇到了一个medium的题:

给你一个数组，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。

示例 1:
```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

示例 2:
```输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```
提示
- 1 <= nums.length <= $10^5$
- -$2^{31}$ <= nums[i] <= $2^{31}$ - 1 
- 0 <= k <= $10^5$

### 头插法
#### 自己写的
```java
public static void rotate(int[] nums, int k) {
    int index = nums.length -1 ;
    for (int i = 0; i < k; i++) {
      int temp = nums[index];
      for (int j = index; j >=0; j--) {
          if(j == 0){
            nums[j] = temp;
          }else{
            nums[j] = nums[j-1];
          }
      }
    }
  }
```

测试通过了，就把代码copy到leetcode上跑用例，发现到最后数组很大的情况的时候，会超过限定的时间，然后去题解中看下其他人的答案。如下

#### 取余数
```java
public static void rotate3(int[] nums, int k){
    int n = nums.length;
    int [] result = new int [n];
    for (int i = 0; i < n; i++) {
      // (i + k ) % n 太聪明了。。
      result[(i + k ) % n] = nums[i];
    }
    System.arraycopy(result, 0, nums, 0, n);
  }
```

### 反转法（空间复杂度的O(1)）
|操作|	结果	|
| --- | --- |
| 原始数组 | 1 2 3 4 5 6 7 |
| 翻转所有元素	 | 7 6 5 4 3 2 1 |
| 翻转 [0, k\bmod n - 1][0,kmodn−1] 区间的元素 | 5 6 7 4 3 2 1 |
| 翻转 [k\bmod n, n - 1][kmodn,n−1] 区间的元素 | 	5~6~7~1~2~3~45 6 7 1 2 3 4 |

```java
public static void rotate(int[] nums, int k){
    // 不做这个的话 k可能会比数组的length大，会报错
    k %= nums.length;
    reverse(nums,0,nums.length - 1);
    reverse(nums,0,k - 1);
    reverse(nums,k,nums.length - 1);
    System.out.println(nums);
  }

  public static void reverse(int [] nums, int start ,int end ){
      while(start < end){
        int temp = nums [end];
        nums[end] = nums[start];
        nums[start] = temp;
        end--;
        start++;
      }
  }
```
