# Two Sum

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
