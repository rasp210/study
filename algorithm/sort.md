# 几种常见的排序算法

下面介绍几种常见的**内部排序算法**，即数据记录在内存中进行排序。

各种排序算法比较：
算法|平均时间复杂度|最坏时间复杂度|最好时间复杂度|辅助空间|稳定性
----|----|----|----|----|----
冒泡排序|O(n^2)|O(n^2)|O(n)|O(1)|稳定

## 冒泡排序
平均时间复杂度：O(n^2)，最好情况：O(n)，最坏情况：O(n^2)，辅助空间：O(1)

稳定，升序排序算法如下：
```c
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

稳定，升序算法：
```c
#include <stdio.h>

void swap(int A[], int i, int j) {
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void cocktailSort(int A[], int n) {
    int left = 0;
    int right = n - 1;
    while (left < right) {
        for (int i = left; i < right; i++) {
            if (A[i] > A[i + 1]) {
                swap(A, i, i + 1);
            }
        }
        right--;
        for (int j = right; j > left; j--) {
            if (A[j] < A[j - 1]) {
                swap(A, j, j - 1);
            }
        }
        left++;
    }
}

int main(void) { 
	int A[] = {3, 4, 7, 1, 2, 8, 9, 5, 6};
	
	int n = sizeof(A) / sizeof(int);
	
	cocktailSort(A, n);
	
	printf("The output is: ");
	for (int i = 0; i < n; i++) {
	    printf("%d ", A[i]);
	}
	printf("\n");
	return 0;
}
```

## 选择排序

不稳定，升序算法：
```c
#include <stdio.h>

void swap(int A[], int i, int j) {
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void selectionSort(int A[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min = i;
        for (int j = i + 1; j < n; j++) {
            if (A[j] < A[min]) {
                min = j;
            }
        }
        if (min != i) {
            swap(A, min, i);
        }
    }
}

int main(void) { 
	int A[] = {3, 4, 7, 1, 2, 8, 9, 5, 6};
	
	int n = sizeof(A) / sizeof(int);
	
	selectionSort(A, n);
	
	printf("The output is: ");
	for (int i = 0; i < n; i++) {
	    printf("%d ", A[i]);
	}
	printf("\n");
	return 0;
}
```

## 插入排序

稳定，升序算法：
```c
#include <stdio.h>

void swap(int A[], int i, int j) {
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void insertionSort(int A[], int n) {
    for (int i = 1; i < n; i++) {
        int j = i - 1;
        int target = A[i];
        while (j >= 0 && target < A[j]) {
            A[j + 1] = A[j];
            j--;
        }
        A[j + 1] = target;
    }
}

int main(void) { 
	int A[] = {3, 4, 7, 1, 2, 8, 9, 5, 6};
	
	int n = sizeof(A) / sizeof(int);
	
	insertionSort(A, n);
	
	printf("The output is: ");
	for (int i = 0; i < n; i++) {
	    printf("%d ", A[i]);
	}
	printf("\n");
	return 0;
}
```

## 希尔排序
稳定   
Space：O(n)   
Time：O(nlogn)   
```c
#include <stdio.h>

void merge(int A[], int left, int mid, int right) {
    int i = left, j = mid + 1;
    int len = right - left + 1;
    int k = 0;
    int* temp = new int[len];
    while (i <= mid && j <= right) {
        temp[k++] = A[i] > A[j] ? A[j++] : A[i++];
    }
    while (i <= mid) {
        temp[k++] = A[i++];
    }
    while (j <= right) {
        temp[k++] = A[j++];
    }
    for (int m = 0; m < len; m++) {
        A[left++] = temp[m];
    }
}

void mergeSort(int A[], int left, int right) {
    if (left == right) {
        return;
    }
    int mid = (left + right) / 2;
    mergeSort(A, left, mid);
    mergeSort(A, mid + 1, right);
    merge(A, left, mid, right);
}

int main(void) { 
    int A[] = {2, 3, 6, 8, 1, 9, 5, 7};
    int len = sizeof(A) / sizeof(int);
    mergeSort(A, 0, len - 1);
    for (int i = 0; i < len; i++) {
        printf("%d ", A[i]);
    }
    printf("\n");
    return 0;
}
```

## 归并排序


## 堆排序


## 快速排序


参考文章：
[1.常用排序算法总结](https://mp.weixin.qq.com/s/ruVOK3iwyPuDjxSTs7JFvA)
