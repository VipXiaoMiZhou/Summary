## 希尔排序

---
希尔排序是一种插入排序算法，它出自D.L.Shell，因此而得名。Shell排序又称作缩小增量排序。

### 算法原理
先取一个小于n的整数d1作为第一个增量，把文件的全部记录分成d1个组。所有距离为dl的倍数的记录放在同一个组中。
先在各组内进行直接插入排序；然后，取第二个增量d2 < d1重复上述的分组和排序，直至所取的增量dt=1(dt < dt-l<；… < d2 < d1），即所有记录放在同一组中进行直接插入排序为止。

### 算法实现（java）

```java
public class ShellSort {

    public void sort(int[] nums) {
        shellSort(nums, nums.length);
    }

    private void shellSort(int[] nums, int n) {
        int inc = n;
        while (inc > 1) {
            inc = inc / 3 + 1;           //计算增量
            shellInsert(nums, n, inc);   //分组插入排序
        }
    }

    private void shellInsert(int nums[], int n, int dk) {

        int i, j, temp;
        for (i = dk; i < n; i++) {
            temp = nums[i];
            for (j = i - dk; (j >= 0) && nums[j] > temp; j -= dk)
                nums[j+dk] = nums[j];
            if (j != i - dk)
                nums[j + dk] = temp;
        }
    }
}

```

### 算法分析

希尔排序的时间复杂度与增量的选取有关。例如，当增量为1时，希尔排序退化成了直接插入排序，此时的时间复杂度为O(N²)，而Hibbard增量的希尔排序的时间复杂度为O(N3/2)。
### 算法稳定性

插入排序是在一个已经有序的小序列的基础上，一次插入一个元素。当然，刚开始这个有序的小序列只有1个元素，就是第一个元素。比较是从有序序列的末尾开始，也就是想要插入的元素和已经有序的最大者开始比起，如果比它大则直接插入在其后面，否则一直往前找直到找到它该插入的位置。如果碰见一个和插入元素相等的，那么插入元素把想插入的元素放在相等元素的后面。所以，相等元素的前后顺序没有改变，从原无序序列出去的顺序就是排好序后的顺序，所以插入排序是稳定的。