# Array
## [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/description/)

Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.

**Example 1:**
```
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12-5-6+50)/4 = 51/4 = 12.75
```

**Note:**

1. <= `k` <= `n` <= 30,000.
2. Elements of the given array will be in the range [-10,000, 10,000].


**Algorithm**

We know that in order to obtain the **averages** of subarrays with length k, we need to obtain the **sum** of these k length subarrays.Assume that we already know the sum of elements from index **i** to index **i+k**, say it is **x**.
Now, to determine the sum of elements from the index **i+1** to the index **i+k+1**, all we need to do is to subtract the element **nums[i]** from **x** and to add the element **nums[i+k+1]** to **x**. 

**JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findMaxAverage = function(nums, k) {
    var sum = 0;
    for(var j = 0; j < k; j++){
        sum += nums[j];
    }
    var res = sum;    
    for(var i = k; i < nums.length; i++){
        sum += nums[i] - nums[i-k];
        res = Math.max(res,sum);
    }
    return res/k;
};
```

**Complexity Analysis**

- Time complexity : O(n). We iterate over the given numsnums array of length n once only.
- Space complexity : O(1). Constant extra space is used.

**My mistake**

The point is the **max sum**, not where we obtain the max sum. So there is no need to care about where the k length subarrays of the max sum is.

## [611. Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number/description/)

Given an array consists of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

**Example 1:**
```
Input: [2,2,3,4]
Output: 3
Explanation:
Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```
**Note:**

1.The length of the given array won't exceed 1000.
2.The integers in the given array are in the range of [0, 1000].

**Algorithm**

****

**Complexity Analysis**
