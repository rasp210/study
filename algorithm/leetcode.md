# 1.Two Sum

>Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
>You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

**Example:**

>Given nums = [2, 7, 11, 15], target = 9,
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].

**Solution**
```
/**
 * Note: The returned array must be malloced, assume caller calls free().
 * O(n*n) 52ms
 * T(1)
 * beats 82.97%
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
>
>Given "bbbbb", the answer is "b", with the length of 1.
>
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Solution**

>Time: 24ms
>Space: 128
>Defeat: 53.9%

```c
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
