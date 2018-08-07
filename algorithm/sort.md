# 几个常见的排序算法

下面介绍几种常见的**内部排序算法**，即数据记录在内存中进行排序。

## 冒泡排序
平均时间复杂度：O(n^2)，最好情况：O(n)，最坏情况：O(n^2)，辅助空间：O(1)
稳定，升序排序算法如下：
```
#include <stdio.h>

void swap(int A[], int i, int j) {
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void bubbleSort(int A[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (A[j] > A[j + 1]) {
                swap(A, j, j + 1);
            }
        }
    }
}

int main(void) { 
	int A[] = {3, 4, 7, 1, 2, 8, 9, 5, 6};
	
	int n = sizeof(A) / sizeof(int);
	
	bubbleSort(A, n);
	
	printf("The output is: ");
	for (int i = 0; i < n; i++) {
	    printf("%d ", A[i]);
	}
	printf("\n");
	return 0;
}
```

## 鸡尾酒排序
## 选择排序
## 插入排序
## 归并排序
## 堆排序
## 快速排序
