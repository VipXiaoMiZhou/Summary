## 计数排序

---
计数排序是一个非基于比较的排序算法，该算法于1954年由 Harold H. Seward 提出。它的优势在于在对一定范围内的整数排序时，它的复杂度为Ο(n+k)（其中k是整数的范围），快于任何比较排序算法。 当然这是一种牺牲空间换取时间的做法，而且当O(k)>O(n*log(n))的时候其效率反而不如基于比较的排序（基于比较的排序的时间复杂度在理论上的下限是O(n*log(n)), 如归并排序，堆排序）

### 算法原理

计数排序的基本思想是对于给定的输入序列中的每一个元素x，确定该序列中值小于x的元素的个数（此处并非比较各元素的大小，而是通过对元素值的计数和计数值的累加来确定）。一旦有了这个信息，就可以将x直接存放到最终的输出序列的正确位置上。例如，如果输入序列中只有17个元素的值小于x的值，则x可以直接存放在输出序列的第18个位置上。
对于大小相同的元素，需要做些特殊处理以保证稳定性。

1. 获取数列中最大的元素
2. 初始化数组c并统计原数组中各元素的个数到c
3. 计算统计完以后的数组c在A中的位置
4. 将数组c中的元素恢复到b中

```java
public class CountSort {

    public int[] sort(int[] nums) {
        countSort(nums, null, getMaxNumber(nums));
    }

	// 获取数组中最大的元素
    private int getMaxNumber(int[] nums) {  
        int max = 0;
        for (int n : nums) {
            if (n > max) {
                max = n;
            }
        }
        return max;
    }

    private void countSort(int[] nums, int[] b, final int k) {
        int[] c = new int[1 + k];  //实例化一个临时数组c

        b = new int[nums.length];  //实例化一个临时数组b

		//统计待排序数组的元素的个数
        for (int i = 0; i < nums.length; i++) {
            c[nums[i]]++;
			//[1, 3, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1]
        }
		
		
		//计算统计完以后的数组B应该在A中的位置
		/**
		*举个例子，现有 nums={1,2,3,3,4}，那么此刻 c = {0,1,1,3,1}，即nums中有0个0,1个1,1个2,2个3,1个4。那么c下标对应的元素对应在nums中的位置就应该是从c[i]=c[i] + c[i-1]。例如，c[1] = c[1] + c[0] = 1 ，c[1]对应的下标1就应该是应该是nums中的第1个元素。
		*
		*/
        for (int i = 1; i <= k; i++) {
            c[i] += c[i - 1];
			//[1, 3, 4, 5, 6, 7, 8, 9, 9, 9, 9, 9, 10]
        }

		
		
        for (int i = nums.length - 1; i >= 0; i--) {
            b[c[nums[i]] - 1] = nums[i]; //把数据放在指定位置。因为 b[c[nums[i]] - 1]的值就是不比他大数据的个数。
            c[nums[i]]--;   			 //因为有相同数据的可能，所以要把该位置数据个数减一
        }

        for (int i = 0; i < nums.length; i++) {
            nums[i] = b[i];
        }

    }

    public static void main(String[] args) {
        int[] nums = { 0, 1, 1, 3, 4, 5, 6, 7, 12, 2 };
        /**
         * 
         * [1, 3, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1]
         */
        Sort sort = new CountSort();
        sort.sort(nums);

        for (int n : nums) {
            System.out.print(n + " ");
        }
    }

}

```
### 算法分析
计数排序对输入的数据有附加的限制条件

1. aa输入的线性表的元素属于有限偏序集S
2. 设输入的线性表的长度为n，|S|=k（表示集合S中元素的总数目为k），则k=O(n)。
 
在这两个条件下，计数排序的复杂性为O(n)。

### 算法稳定性

技术排序算法是一种稳定的排序算法
