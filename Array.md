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

The condition for the triplets `(a, b, c)` representing the lengths of the sides of a triangle, to form a valid triangle, is that the sum of any two sides should always be greater than the third side alone. i.e. `a + b > c, b + c > a, a + c > b`.
If we sort the given nums array first, that is to say, a<=b<=c, then we have b+c>a and a+c>b. All we need to know is how many a+b>c can suffice. Assume i,j,k are the index of a,b,c. We need to find the range of k to make c valid, then it is obvious that `nums[k]` starts from j+1. And obviously there will be a `nums[k]` which is too large that it will make `nums[i]+nums[j] < nums[k]`. Thus, there will exist a right limit on the value of index `k`, such that the elements satisfy `nums[k] > nums[i] + nums[j]`.Thus, if we are able to find this right limit value of `k`(indicating the element just greater than `nums[i] + nums[j]`), we can conclude that all the elements in nums array in the range `(j+1, k-1)`(both included) satisfy the required inequality. Thus, the count of elements satisfying the inequality will be given by `(k-1) - (j+1) + 1 = k - j - 1`.
Further, when we choose a higher value of index `j` for a particular `i` chosen, we need not start from the index `j + 1`. Instead, we can start off directly from the value of `k` where we left for the last index `j`. This helps to save redundant computations.

**JavaScript**
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var triangleNumber = function(nums) {
    function sortNumber(a,b){
        return a - b
    }
    var nums1 = nums.sort(sortNumber);
    var count = 0 ;
    // console.log(nums.sort());
    for(var i = 0; i < nums1.length - 2 ; i++){
        var k = i + 2;
        for(var j = i + 1 ; j < nums1.length - 1 && nums1[i] !== 0 ; j++){
            while( k < nums1.length && nums1[i]+nums1[j] > nums1[k])
                k++;
            count += k-j-1;
        }
    }
    return count;
};
```

**Complexity Analysis**
- Time complexity : `O(n^2)` Loop of k and j will be executed `O(n^2)` times in total, because, we do not reinitialize the value of k for a new value of j chosen(for the same i). Thus the complexity will be O(n^2+n^2)=O(n^2).
- Space complexity : O(logn). Sorting takes O(logn) space.

## [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/description/)
Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.

Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number n, return if n new flowers can be planted in it without violating the no-adjacent-flowers rule.

**Example 1:**
```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```
**Example 2:**
```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False
```
**Note:**
1. The input array won't violate no-adjacent-flowers rule.
2. The input array size is in the range of [1, 20000].
3. n is a non-negative integer which won't exceed the input array size.

**Algorithm**

1. When constant 0 is 1/2, we can plant 0 new flower;
2. When constant 0 is 3/4, we can plant 1 new flower;
3. When constant 0 is 5/6, we can plant 2 new flower...
4. So, we can see When constant 0 is `empty`, we can plant integer `(empty-1)/2` new flower. What we need to do is finding how many constant 0 there is.

**JavaScript**
```javascript
/**
 * @param {number[]} flowerbed
 * @param {number} n
 * @return {boolean}
 */
var canPlaceFlowers = function(flowerbed, n) {
    var i = 0;
    var newFlower = 0;
    flowerbed = ([1,0].concat(flowerbed)).concat([0,1]);  
    while(i < flowerbed.length){
        if(flowerbed[i] === 0){
            var empty = 0;
            var j = i;
            while(flowerbed[j] === 0){
                j++;
                empty ++;
            }
            newFlower += parseInt((empty - 1)/2);  
            i = i + empty;
        }else{
            i++;
        }
    } 
    return n <= newFlower ? true : false ; 
};
```

**Complexity Analysis**
- Time complexity : O(n). A single scan of the flowerbed array of size n is done.
- Space complexity : O(1). Constant extra space is used.

**Details**
- When constant 0 appears at `index[0]` or `index[length-1]`, for example, `[0,0,1,0,0]`, it is possible that we can plant 2 more new flower. In this circumstance, we can add `[1,0]` to the beginning of the given array flowerbed, add `[0,1]` to the end of flowerbed. It will not only has no effect on the results, but also can help simplify the judgement.
- JavaScript use parseInt to get the integer of Division Operation(/).
- When do the judgement operation, use '==='.
