# 数组循环右移

>【描述】将一个长度为 n 的数组 A，循环右移 k 位   
>【示例】  
>数组：1,2,3,4,5,6,7,8,9  
>长度 n：9  
>循环右移 k：3 
>右移后的数组：7,8,9,1,2,3,4,5,6  

## 方法一：

循环 k 次，每次移动 n 个值。

```
#include <stdio.h>

void rightShift(int A[], int n, int k) {
    if (k <= 0) {
        return;
    }
    if (n <= 0) {
        return;
    }
    while (k--) {
        int temp = A[n - 1];
        for (int i = n - 1; i > 0; i--) {
            A[i] = A[i - 1];
        }
        A[0] = temp;
    }
}

void printArr(int A[], int n) {
    for (int i = 0; i < n; i++) {
        printf("%d ", A[i]);
    } 
    printf("\n");
}

int main(void) { 
    int A[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    //int A[] = {};
    int n = sizeof(A) / sizeof(int);
    //int n = -1;
    int k = 3;
    //int k = -2;
	printf("pre list is: ");
	printArr(A, n);
	rightShift(A, n, k);
	printf("now list is: ");
	printArr(A, n);
	return 0;
}
```

## 方法二：

前 n-k 个数反转，后 k 个数反转，拼接在一起后反转。

```
#include <stdio.h>

void swap(int A[], int i, int j) {
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void reverse(int A[], int left, int right) {
    if (left >= right) {
        return;
    }
    
    for (; left < right; left++, right--) {
        swap(A, left, right);
    }
}

void rightShift(int A[], int n, int k) {
    if (k <= 0) {
        return;
    }
    if (n <= 0) {
        return;
    }
    reverse(A, 0, n - k - 1);
    reverse(A, n - k, n - 1);
    reverse(A, 0, n - 1);
}

void printArr(int A[], int n) {
    for (int i = 0; i < n; i++) {
        printf("%d ", A[i]);
    } 
    printf("\n");
}

int main(void) { 
    int A[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    //int A[] = {};
    int n = sizeof(A) / sizeof(int);
    //int n = -1;
    int k = 3;
    //int k = -2;
	printf("pre list is: ");
	printArr(A, n);
	rightShift(A, n, k);
	printf("now list is: ");
	printArr(A, n);
	return 0;
}
```
