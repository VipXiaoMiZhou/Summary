
## 冒泡排序



---

一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。

### 算法原理



1. 比较两个相邻元素，如果前一个比后一个大（小），交换。

2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。

3. 针对所有的元素重复以上的步骤，除了最后一个。

4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较



### 算法实现(java实现)

```java

public class BubbleSort implements Sort {

    public int[] sort(int nums[]) {
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len - 1 - i; j++) {
                if (nums[j] > nums[j + 1]) {
                    int n = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = n;
                }
            }
        }
        return nums;
    }
}

```



### 算法分析

时间复杂度：O(n^2)

### 算法稳定性

冒泡排序就是把小的元素往前调或者把大的元素往后调。比较是相邻的两个元素比较，交换也发生在这两个元素之间。所以，如果两个元素相等，位置就不会发生变化；如果两个相等的元素没有相邻，那么即使通过前面的两两交换把两个相邻起来，这时候也不会交换，所以相同元素的前后顺序并没有改变，所以冒泡排序是一种稳定排序算法。















