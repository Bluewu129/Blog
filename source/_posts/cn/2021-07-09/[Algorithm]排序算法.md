---
title: '[Algorithm]排序算法'
catalog: true
date: 2021-07-09 10:34:17
subtitle: Sorting Algorithms
lang: cn
header-img: /img/posts/3.jpg
tags:
- Java
- Data Structures
- Algorithms
categories:
- Data Structures and Algorithms
---

---
### 0. 算法描述
#### 0.1 算法分类
排序算法可以分为内部排序和外部排序，内部排序是数据记录在内存中进行排序，而外部排序是因排序的数据很大，一次不能容纳全部的排序记录，在排序过程中需要访问外存。常见的内部排序算法有：**插入排序、希尔排序、选择排序、冒泡排序、归并排序、快速排序、堆排序、基数排序**等。用一张图概括：![image.png](1.png)

#### 0.2 概念
+ 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面。
+ 不稳定：如果a原本在b的前面，而a=b，排序之后 a 可能会出现在 b 的后面。
+ 时间复杂度与空间复杂度：[[Algorithm]时间复杂度与空间复杂度](https://blue129.com/archives/algorithm%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6%E4%B8%8E%E7%A9%BA%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6)
+ n：数据规模
+ k：“桶”的个数
+ In-place：占用常数内存，不占用额外内存
+ Out-place：占用额外内存

### 1. 冒泡排序（Bubble Sort）
冒泡排序是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。 

#### 1.1 算法描述
+ 比较相邻的元素。如果第一个比第二个大，就交换它们两个;
+ 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数;
+ 针对所有的元素重复以上的步骤，除了最后一个;
+ 重复步骤1~3，直到排序完;
#### 1.2 动图演示
![BubbleSort.gif](2.gif)
#### 1.3 代码实现
```Java
public void bubbleSort() {
    int arr[] = {8, 5, 3, 2, 4};

    for (int i = 0; i < arr.length; i++) {
      for (int j = 0; j < arr.length - i - 1; j++) {
        if (arr[j] > arr[j + 1]) {
          int temp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = temp;
        }
      }
    }
  }

public class BubbleSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        for (int i = 1; i < arr.length; i++) {
            // 设定一个标记，若为true，则表示此次循环没有进行交换，也就是待排序列已经有序，排序已经完成。
            boolean flag = true;

            for (int j = 0; j < arr.length - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;

                    flag = false;
                }
            }

            if (flag) {
                break;
            }
        }
        return arr;
    }
}

```

### 2. 选择排序（Selection Sort）
选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。 
#### 2.1 算法描述
n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：
+ 初始状态：无序区为R[1..n]，有序区为空；
+ 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
+ n-1趟结束，数组有序化了。
#### 2.2 动图演示
![selectionSort.gif](3.gif)
#### 2.3 代码实现
```Java
public  void selectionSort() {
    int arr[] = {8, 5, 3, 2, 4};

    //选择
    for (int i = 0; i < arr.length; i++) {
      // 默认第一个最小
      int min = arr[i];
      //记录最小的下标
      int index = i;
      //与后边的数据对比，得出最小值和下标
      for (int j = i + 1; j < arr.length; j++) {
        if (min > arr[j]) {
          min = arr[j];
          index = j;
        }
      }
      //然后将最小值与本次循环的，开始值交换
      int temp = arr[i];
      arr[i] = min;
      arr[index] = temp;
      //说明：将i前面的数据看成一个排好的队列，i后面的看成一个无序队列。每次只需要找无需的最小值，做替换
    }
  }

public class SelectionSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        // 总共要经过 N-1 轮比较
        for (int i = 0; i < arr.length - 1; i++) {
            int min = i;

            // 每轮需要比较的次数 N-i
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[min]) {
                    // 记录目前能找到的最小值元素的下标
                    min = j;
                }
            }

            // 将找到的最小值和i位置所在的值进行交换
            if (i != min) {
                int tmp = arr[i];
                arr[i] = arr[min];
                arr[min] = tmp;
            }

        }
        return arr;
    }
}

```
#### 2.4 算法分析
表现最稳定的排序算法之一，因为无论什么数据进去都是 **O(n2)** 的时间复杂度，所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。理论上讲，选择排序可能也是平时排序一般人想到的最多的排序方法了吧。

### 3. 插入排序（Insertion Sort）
插入排序（Insertion-Sort）的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

#### 3.1 算法描述
一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：

+ 从第一个元素开始，该元素可以认为已经被排序；
+ 取出下一个元素，在已经排序的元素序列中从后向前扫描；
+ 如果该元素（已排序）大于新元素，将该元素移到下一位置；
+ 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
+ 将新元素插入到该位置后；
+ 重复步骤2~5。

#### 3.2 动图演示
![insertionSort.gif](4.gif)

#### 3.3 代码实现
```java
public void insertionSort() {
    int arr[] = {7, 5, 3, 2, 4};

    //插入排序
    for (int i = 1; i < arr.length; i++) {
      //外层循环，从第二个开始比较
      for (int j = i; j > 0; j--) {
        //内存循环，与前面排好序的数据比较，如果后面的数据小于前面的则交换
        if (arr[j] < arr[j - 1]) {
          int temp = arr[j - 1];
          arr[j - 1] = arr[j];
          arr[j] = temp;
        } else {
          //如果不小于，说明插入完毕，退出内层循环
          break;
        }
      }
    }
  }

public class InsertSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        // 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有序的
        for (int i = 1; i < arr.length; i++) {

            // 记录要插入的数据
            int tmp = arr[i];

            // 从已经排序的序列最右边的开始比较，找到比其小的数
            int j = i;
            while (j > 0 && tmp < arr[j - 1]) {
                arr[j] = arr[j - 1];
                j--;
            }

            // 存在比其小的数，插入
            if (j != i) {
                arr[j] = tmp;
            }

        }
        return arr;
    }
}

```
#### 3.4 算法分析
插入排序在实现上，通常采用 **in-place** 排序（即只需用到 **O(1)** 的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。 

### 4. 希尔排序（Shell Sort）
1959年Shell发明，第一个突破 **O(n2)** 的排序算法，是简单插入排序的改进版。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序,但希尔排序是非稳定排序算法。

#### 4.1 算法描述
先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：
+ 选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1；
+ 按增量序列个数k，对序列进行k 趟排序；
+ 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

#### 4.2 动图演示
![ShellSort.gif](5.gif)
#### 4.3 代码实现
```java
public void shellSort() {
    int arr[] = {7, 5, 3, 2, 4};

    //希尔排序（插入排序变种版）
    for (int i = arr.length / 2; i > 0; i /= 2) {
      //i层循环控制步长
      for (int j = i; j < arr.length; j++) {
        //j控制无序端的起始位置
        for (int k = j; k > 0 && k - i >= 0; k -= i) {
          if (arr[k] < arr[k - i]) {
            int temp = arr[k - i];
            arr[k - i] = arr[k];
            arr[k] = temp;
          } else {
            break;
          }
        }
      }
      //j,k为插入排序，不过步长为i
    }
  }

public class ShellSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        int gap = 1;
        while (gap < arr.length/3) {
            gap = gap * 3 + 1;
        }

        while (gap > 0) {
            for (int i = gap; i < arr.length; i++) {
                int tmp = arr[i];
                int j = i - gap;
                while (j >= 0 && arr[j] > tmp) {
                    arr[j + gap] = arr[j];
                    j -= gap;
                }
                arr[j + gap] = tmp;
            }
            gap = (int) Math.floor(gap / 3);
        }

        return arr;
    }
}

```

#### 4.4 算法分析
希尔排序是基于插入排序的以下两点性质而提出改进方法的：
+ 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率；
+ 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位；

希尔排序的基本思想是：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序。
希尔排序的核心在于间隔序列的设定。既可以提前设定好间隔序列，也可以动态的定义间隔序列。动态定义间隔序列的算法是《算法（第4版）》的合著者Robert Sedgewick提出的。　

### 5. 归并排序（Merge sort）
归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。 

#### 5.1 算法描述
+ 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
+ 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
+ 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
+ 重复步骤 3 直到某一指针达到序列尾；
+ 将另一序列剩下的所有元素直接复制到合并序列尾。

#### 5.2 动图演示
![mergeSort.gif](6.gif)

#### 5.3 代码实现
```java
public static void main(String[] args) {

        int arr[] = {7, 5, 3, 2, 4, 1，6};

        //归并排序
        int start = 0;
        int end = arr.length - 1;
        mergeSort(arr, start, end);
    }

    public static void mergeSort(int[] arr, int start, int end) {
        //判断拆分的不为最小单位
        if (end - start > 0) {
            //再一次拆分，知道拆成一个一个的数据
            mergeSort(arr, start, (start + end) / 2);
            mergeSort(arr, (start + end) / 2 + 1, end);
            //记录开始/结束位置
            int left = start;
            int right = (start + end) / 2 + 1;
            //记录每个小单位的排序结果
            int index = 0;
            int[] result = new int[end - start + 1];
            //如果查分后的两块数据，都还存在
            while (left <= (start + end) / 2 && right <= end) {
                //比较两块数据的大小，然后赋值，并且移动下标
                if (arr[left] <= arr[right]) {
                    result[index] = arr[left];
                    left++;
                } else {
                    result[index] = arr[right];
                    right++;
                }
                //移动单位记录的下标
                index++;
            }
            //当某一块数据不存在了时
            while (left <= (start + end) / 2 || right <= end) {
                //直接赋值到记录下标
                if (left <= (start + end) / 2) {
                    result[index] = arr[left];
                    left++;
                } else {
                    result[index] = arr[right];
                    right++;
                }
                index++;
            }
            //最后将新的数据赋值给原来的列表，并且是对应分块后的下标。
            for (int i = start; i <= end; i++) {
                arr[i] = result[i - start];
            }
        }
    }
```
#### 5.4 算法分析
归并排序是一种稳定的排序方法。和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是 **O(nlogn)** 的时间复杂度。代价是需要额外的内存空间。

### 6. 快速排序（Quick Sort）
快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。
### 6.1 算法描述
快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：
+ 从数列中挑出一个元素，称为 “基准”（pivot）；
+ 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
+ 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。
### 6.2 动图演示
![QuickSort.gif](7.gif)
### 6.3 代码实现
```java
public class QuickSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        return quickSort(arr, 0, arr.length - 1);
    }

    private int[] quickSort(int[] arr, int left, int right) {
        if (left < right) {
            int partitionIndex = partition(arr, left, right);
            quickSort(arr, left, partitionIndex - 1);
            quickSort(arr, partitionIndex + 1, right);
        }
        return arr;
    }

    private int partition(int[] arr, int left, int right) {
        // 设定基准值（pivot）
        int pivot = left;
        int index = pivot + 1;
        for (int i = index; i <= right; i++) {
            if (arr[i] < arr[pivot]) {
                swap(arr, i, index);
                index++;
            }
        }
        swap(arr, pivot, index - 1);
        return index - 1;
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}

```
### 6.4 算法分析
快速排序的名字起的是简单粗暴，因为一听到这个名字你就知道它存在的意义，就是快，而且效率高！它是处理大数据最快的排序算法之一了。虽然 Worst Case 的时间复杂度达到了 O(n²)，但是人家就是优秀，在大多数情况下都比平均时间复杂度为 O(n logn) 的排序算法表现要更好，可是这是为什么呢，我也不知道。好在我的强迫症又犯了，查了 N 多资料终于在《算法艺术与信息学竞赛》上找到了满意的答案：

+ 快速排序的最坏运行情况是 O(n²)，比如说顺序数列的快排。但它的平摊期望时间是 O(nlogn)，且 O(nlogn) 记号中隐含的常数因子很小，比复杂度稳定等于 O(nlogn) 的归并排序要小很多。所以，对绝大多数顺序性较弱的随机数列而言，快速排序总是优于归并排序。


---
```java
// TODO 补充ing
```
