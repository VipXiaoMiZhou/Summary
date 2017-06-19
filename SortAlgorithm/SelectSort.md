### 选择排序

---
选择排序是一种简单直观的算法，顾名思义，选择算法就是从剩下未排序的序列中选出最大（最小）的元素依次排到放到序列开始的位置。

### 算法原理

1. 遍历数组，记下当前元素的值。 
2. 从该元素之后开始遍历剩下的元素，获取这些元素的最大（小）值以及位置。
3. 将遍历的到的最大（小）值和当前值比较，如果满足排序的要求，则调换。
4. 重复1直到排序完成


### 算法实现（java）

```java

public class SelectSort {
    public int[] sort(int nums[]) {

        for (int i = 0; i < nums.length; i++) {
            int min = nums[i], min_index = i;

            // get the minimum item form remains array
            for (int j = i; j < nums.length; j++) {

                if (nums[j] < min) {
                    min = nums[j];
                    min_index = j;
                }
            }

            // replace
            int tmpValue = nums[i];
            nums[i] = min;
            nums[min_index] = tmpValue;

        }

        return nums;
    }
}

```

### 算法分析

时间复杂度: O(n^2)

### 算法稳定性

选择排序是不稳定的排序方法（比如序列[5， 5， 3]第一次就将第一个[5]与[3]交换，导致第一个5挪动到第二个5后面;















