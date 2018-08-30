# 1.Two Sum

>Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.   
>You may assume that each input would have **exactly** one solution, and you may not use the same element twice.   

**Example**   
>Given nums = [2, 7, 11, 15], target = 9,   
>Because nums[0] + nums[1] = 2 + 7 = 9,   
>return [0, 1].   

**Solution**   
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target) {
    int* ret = malloc(2 * sizeof(int)); // 分配内存空间
    if (numsSize < 2) {
        return ret;
    }
    
    int i, j;
    int left;
    for (i = 0; i < numsSize - 1; i++) {
        left = target - nums[i];
        for (j = i + 1; j < numsSize; j++) {
            if (left == nums[j]) {
                ret[0] = i;
                ret[1] = j;   
            }
        }
    }
    return ret;
}
```

# 3.Longoest Substring Without Repeating Characters

>最长无重复字符子串   
>Given a string, find the length of the longest substring without repeating characters.   

**Example**
>Given "abcabcbb", the answer is "abc", which the length is 3.   
>Given "bbbbb", the answer is "b", with the length of 1.   
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.   

**Solution**
```c
/**
 * 1.用一个int类型的数组map来存储字符-索引
 * 2.用两个索引分别指向当前无重复子串的最左left和最右right索引
 * 3.left和right只增不减
 * 4.left取left和命中map中的索引的较大值
 * 5.right每次循环+1
 * 6.每次计算子串的长度取最大值
 */
#include <stdio.h>
#include <string.h>

int lengthOfLongestSubstring(char* s) {
    int len = strlen(s);
    if (len <= 1) {
        return len;
    }
    int map[128] = {0};
    int left = 1, right = 1, maxLen = 0;
    for (; right <= len; right++) {
        if (map[s[right - 1]] && map[s[right - 1]] + 1 > left) {
            left = map[s[right - 1]] + 1;
        }
        map[s[right - 1]] = right;
        if (right - left + 1 > maxLen) {
            maxLen = right - left + 1;
        }
    }
    return maxLen;
}

int main(void) { 
    char s[] = "hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789hijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    int max = lengthOfLongestSubstring(s);
	printf("Max=%d", max);
	return 0;
}
```

# 206.Reverse Linked List

>反转单链表
>Reverse a singly linked list.

**Example**   
>Input: 1->2->3->4->5->NULL   
>Output: 5->4->3->2->1->NULL   

**Solution 1**   
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    
    struct ListNode* pre = head;
    struct ListNode* cur = head->next;
    struct ListNode* temp;
    
    while (cur) {
        temp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = temp;
    }
    head->next = NULL;
    return pre;
}
```

**Solution 2**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    
    struct ListNode* newHead = reverseList(head->next);
    head->next->next = head;
    head->next = NULL;
    return newHead;
}
```



# 344.Reverse String

> 反转字符串   
> Write a function that takes a string as input and returns the string reversed.

**Example**
>Input: "hello"   
>Output: "olleh"  
>    
>Input: "A man, a plan, a canal: Panama"   
>Output: "amanaP :lanac a ,nalp a ,nam A"   

**Solution**
```c
char* reverseString(char* s) {
    int len = strlen(s);
    if (len <= 1) {
        return s;
    }
    
    char temp;
    for (int l = 0, r = len -1; l < r; l++, r--) {
        temp = s[l];
        s[l] = s[r];
        s[r] = temp;
    }
    return s;
}
```