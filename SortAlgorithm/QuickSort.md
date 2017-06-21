## 快速排序

---
通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

### 算法原理

设要排序的数组是A[0]……A[N-1]，首先任意选取一个数据（通常选用数组的第一个数）作为关键数据，然后将所有比它小的数都放到它前面，所有比它大的数都放到它后面，这个过程称为一趟快速排序。

举个例子：
假如要排序的序列为：

[6,45,7,3,7,9,2,5]

第一步：选择一个元素作为基准（一般是第一个元素），使用两个指针left，right分别从左右开始扫描，目的是将元素按比基准元素大还是小分为两个部分。

因为 5 < 6,右循环结束，替换
[5,45,7,3,7,9,5]

5 < 6，left++，

45 > 6, 左循环结束，替换
[5,45,7,3,7,9,45]

右循环开始
45 > 6,right--

7 > 6, right--

9 > 6, right--

7 > 6, right--

3 < 6, 右循环结束，替换

[5,3,7,3,7,9,45]

右循环开始

3  < 6 left ++

7 > 6 左循环结束，替换

[5,3,7,7,7,9,45]

循环结束，替换基准到元素列
[5,3,6,7,7,9,45]

至此，一趟分区操作完成。

### 算法实现（java）
```java

public class QuickSort {

    public int[] sort(int[] nums) {
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }

    private int partition(int[] nums, int left, int right) {

        int base = nums[left];

        while (left < right) {
            while (left < right && nums[right] > base) {
                right--;
            }
            nums[left] = nums[right];

            while (left < right && nums[left] <= base) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = base;

        return left;
    }

    private void quickSort(int[] nums, int left, int right) {
        int loc = 0;
        if (left < right) {
            loc = partition(nums, left, right);
            quickSort(nums, left, loc - 1);
            quickSort(nums, loc + 1, right);
        }
    }

    public static void main(String[] args) {
        int[] nums = { 5, 1, 2, 13, 14, -1, 6, 7, 99, 0, 3, 4, 98, 10 };
        // partition(nums,0,12);
        //[6,45,7,3,7,9,2,5]
        QuickSort qs = new QuickSort();

        for (int n : qs.sort(nums)) {
            System.out.print(n + " ");
        }
    }

}

```
### 算法分析

快速排序的时间主要耗费在划分操作上，对长度为k的区间进行划分，共需k-1次关键字的比较。

最坏情况是每次划分选取的基准都是当前无序区中关键字最小(或最大)的记录，划分的结果是基准左边的子区间为空(或右边的子区间为空)，而划分所得的另一个非空的子区间中记录数目，仅仅比划分前的无序区中记录个数减少一个。时间复杂度为O(n^2)

在最好情况下，每次划分所取的基准都是当前无序区的"中值"记录，划分的结果是基准的左、右两个无序子区间的长度大致相等。总的关键字比较次数：O(nlgn)

尽管快速排序的最坏时间为O(n2)，但就平均性能而言，它是基于关键字比较的内部排序算法中速度最快者，快速排序亦因此而得名。它的平均时间复杂度为O(nlgn)。

### 算法稳定性

快速排序不是一种稳定的排序算法，也就是说，多个相同的值的相对位置也许会在算法结束时产生变动。