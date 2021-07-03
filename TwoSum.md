# 1. Two Sum

## Problem
Source: [LeetCode](https://leetcode.com/problems/two-sum/)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

### Example 1:

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

### Example 2:

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```


### Example 3:

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

## Constraints:

- 2 <= nums.length <= 104
- -10^9 <= nums[i] <= 10^9
- -10^9 <= target <= 10^9
- **Only one valid answer exists.**


**Follow-up:** Can you come up with an algorithm that is less than O(n2) time complexity?

---

## Solution

### Approach 1: Brute Force

The brute force approach is simple. Loop through each element xx and find if there is another value that equals to target - `target−x`

#### Scala
```scala
def twoSumBruteForce(nums: Array[Int], target: Int): Array[Int] = {
    (0 until (nums.length - 1)).flatMap(i => {
      ((i + 1) until nums.length).flatMap(j => {
        if (i != j && nums(i) + nums(j) == target)
          Some(Array(i, j))
        else None
      })
    }).headOption match {
      case Some(arr) => arr
      case _ => throw new IllegalArgumentException("No two sum solution")
    }
}
```

#### Complexity Analysis

- Time complexity : O(n^2). For each element, we try to find its complement by looping through the rest of array which takes O(n) time. Therefore, the time complexity is O(n^2).
- Space complexity : O(1).

### Approach 2: Two-pass Hash Table

To improve our run time complexity, we need a more efficient way to check if the complement exists in the array. If the complement exists, we need to look up its index. What is the best way to maintain a mapping of each element in the array to its index? A hash table.

We reduce the look up time from O(n) to O(1) by trading space for speed. A hash table is built exactly for this purpose, it supports fast look up in near constant time. I say "near" because if a collision occurred, a look up could degenerate to O(n) time. But look up in hash table should be amortized O(1) time as long as the hash function was chosen carefully.

A simple implementation uses two iterations. In the first iteration, we add each element's value and its index to the table. Then, in the second iteration we check if each element's complement (target - nums[i]) exists in the table. Beware that the complement must not be nums[i] itself!

#### Scala

```scala
def twoSum2PassHash(nums: Array[Int], target: Int): Array[Int] = {
  val valueKeyMap = nums.indices.map(i => nums(i) -> i).toMap
  nums.indices.flatMap(i => {
    val complement = target - nums(i)
    if (valueKeyMap.contains(complement) && valueKeyMap(complement) != i)
      Some(Array(i, valueKeyMap(complement)))
    else
      None
  }).headOption match {
    case Some(arr) => arr
    case _ => throw new IllegalArgumentException("No two sum solution")
  }
}
```
