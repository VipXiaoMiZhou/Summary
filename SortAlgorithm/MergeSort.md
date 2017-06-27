## 归并排序

---

归并排序（MERGE-SORT）是建立在归并操作上的一种有效的排序算法,该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。
归并过程为：比较a[i]和b[j]的大小，若a[i]≤b[j]，则将第一个有序表中的元素a[i]复制到r[k]中，并令i和k分别加上1；否则将第二个有序表中的元素b[j]复制到r[k]中，并令j和k分别加上1，如此循环下去，直到其中一个有序表取完，
然后再将另一个有序表中剩余的元素复制到r中从下标k到下标t的单元。归并排序的算法我们通常用递归实现，先把待排序区间[s,t]以中点二分，接着把左边子区间排序，再把右边子区间排序，最后把左区间和右区间用一次归并操作合并成有序的区间[s,t]。

### 算法原理

归并操作的工作原理如下:

1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置
4. 重复步骤3直到某一指针超出序列尾
5. 将另一序列剩下的所有元素直接复制到合并序列尾


### 算法实现(java)

```java
public class MergeSort {

    private void merge(int sourceArr[], int tempArr[], int start, int mid, int end) {
        int i = start, j = mid + 1, k = start;
        while (i <= mid && j <= end) {
            if (sourceArr[i] > sourceArr[j])
                tempArr[k++] = sourceArr[j++];
            else
                tempArr[k++] = sourceArr[i++];
        }

        // merge left part
        while (i <= mid)
            tempArr[k++] = sourceArr[i++];

        // merge right part
        while (j <= end)
            tempArr[k++] = sourceArr[j++];
        for (i = start; i <= end; i++)
            sourceArr[i] = tempArr[i];
    }

    public int[] mergeSort(int sourceArr[], int tempArr[], int start, int end) {
        int mid;

        if (start < end) {
            mid = (start + end) / 2;
            mergeSort(sourceArr, tempArr, start, mid);
            mergeSort(sourceArr, tempArr, mid + 1, end);
            merge(sourceArr, tempArr, start, mid, end);
        }
        return sourceArr;
    }

    public static void main(String[] args) {
        int[] nums = { 0, 3, 1, 234, 345, 234234, 234234, 234234 };
        int[] tmpnums = new int[nums.length];
        MergeSort ms = new MergeSort();
        for (int n : ms.mergeSort(nums, tmpnums, 0, nums.length - 1)) {
            System.out.print(n + " ");
        }

    }

}

```

### 算法分析

时间复杂度为O(nlogn) 这是该算法中最好、最坏和平均的时间性能。
空间复杂度为 O(n)
比较操作的次数介于(nlogn) / 2和nlogn - n + 1。
赋值操作的次数是(2nlogn)。
归并排序比较占用内存，但却是一种效率高且稳定的算法。

### 算法稳定性

归并排序算法是一种稳定的排序算法