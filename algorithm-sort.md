# 几种常见的排序算法
下面介绍几种常见的`内部排序算法`，即数据记录在内存中进行排序。

各种排序算法比较：

算法|平均时间复杂度|最坏时间复杂度|最好时间复杂度|辅助空间|稳定性
----|----|----|----|----|----
冒泡排序|O(n^2)|O(n^2)|O(n)|O(1)|稳定
定向冒泡排序|O(n^2)|O(n^2)|O(n)|O(1)|稳定
直接插入排序|O(n^2)|O(n^2)|O(n)|O(1)|稳定
二分查找插入排序|O(n^2)|O(n^2)|O(nlogn)|O(1)|稳定
简单选择排序|O(n^2)|O(n^2)|O(n^2)|O(1)|不稳定
快速排序|O(nlogn)|O(nlogn)|O(n^2)|O(logn)~O(n)|不稳定
堆排序|O(nlogn)|O(nlogn)|O(nlogn)|O(1)|不稳定
归并排序|O(nlogn)|O(nlogn)|O(nlogn)|O(n)|稳定
希尔排序|O(nlogn)~O(n^2)|O(n(logn)^2)|O(nlogn)|O(1)|不稳定

## 冒泡排序
冒泡排序步骤如下：
1. 比较相邻的两个元素，如果前者大于后者，则交换
2. 每遍历一遍，都将获得当前序列的最大值
3. 对剩下的未确定大小的序列重复步骤1～2，直至剩下序列元素个数未1
```java
public class Sort {
    public void bubbleSort(int[] nums) {
        int len = nums.length;
        while (len > 1) {
            for (int i = 1; i < len; i++) {
                if (nums[i - 1] > nums[i]) {
                    swap(nums, i - 1, i);
                }
            }
            len--;
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        if (i != j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
}
```

## 定向冒泡排序(鸡尾酒排序)
定向冒泡排序步骤如下：
1. 跟冒泡排序类似，不同点在于，每次从低到高然后从高到低
```java
public class Sort {
    public void cocktailSort(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            for (int i = left; i < right; i ++) {
                if (nums[i] < nums[i + 1]) {
                    swap(nums, i, i + 1);
                }
            }
            right--;
            for (int j = right; j > left; j--) {
                if (nums[j - 1] < nums[j]) {
                    swap(nums, j - 1, j);
                }
            }
            left++;
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        if (i != j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
}
```

## 直接插入排序
直接插入排序的步骤如下：
1. 序列从做到右分为已排序序列和未排序序列两部分，第0个元素视为已经排序序列，剩下的未未排序序列
2. 从未排序序列中取第一个元素，从右往左遍历已排序序列，找到该元素在已排序序列中的位置
3. 重复步骤1～2，遍历未排序序列，直到未排序序列长度为0
```java
public class Sort {
    public void insertionSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            int currVal = nums[i];
            int j = i - 1;
            while (j >= 0 && nums[j] > currVal) {
                nums[j + 1] = nums[j];
                j--;
            }
            nums[j + 1] = currVal;
        }
    }
}
```

## 二分查找插入排序
二分查找插入排序是先通过二分查找找到目标位置，步骤如下：
1. 同直接插入排序，只不过查找目标位置的方法通过二分查找查询
```java
public class Sort {
    public void binarySearchInsertionSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            int currVal = nums[i];
            int left = 0, right = i - 1;
            while (left <= right) {
                int mid = (left + right) / 2;
                if (nums[mid] > currVal) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            System.arraycopy(nums, left, nums, left + 1, i - left);
            nums[left] = currVal;
        }
    }
}
```

## 简单选择排序
简单选择排序的步骤如下：
1. 遍历数组，每次从数组中找到当前序列的最小值，然后放到左侧最终位置
2. 继续遍历剩余未找到最终位置的序列，重复步骤1
```java
public class Sort {
    public void selectionSort(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            int min = i;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[min]) {
                    min = j;
                }
            }
            swap(nums, min, i);
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        if (i != j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
}
```

## 快速排序
快速排序使用`分治策略(Divide and Conquer)`来把一个序列分为两个子序列。步骤如下：
1. 从序列中挑出一个元素作为基准(pivot)
2. 把所有比基准值小的元素放在基准前面，所有比基准值大的元素放在基准的后面，相同的数可以到任一边，这个过程称为分区(partition)
3. 对每个分区递归的进行步骤1～2，序列结束的条件是序列大小小于2
```java
public class Sort {
    public void quickSort(int[] nums, int left, int right) {
        if (left < right) {
            int pivot = partion(nums, left, right);
            quickSort(nums, left, pivot - 1);
            quickSort(nums, pivot + 1, right);
        }
    }
    
    private int partion(int[] nums, int left, int right) {
        int tail = left - 1;
        int pivotVal = nums[right];
        for (int i = left; i < right; i++) {
            if (nums[i] < pivotVal) {
                swap(nums, ++tail, i);
            }
        }
        swap(nums, ++tail, right);
        return tail;
    }
    
    private void swap(int[] nums, int i, int j) {
        if (i != j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
}
```

## 堆排序

## 归并排序

## 希尔排序


## 参考文章
[1.常用排序算法总结](https://mp.weixin.qq.com/s/ruVOK3iwyPuDjxSTs7JFvA)
