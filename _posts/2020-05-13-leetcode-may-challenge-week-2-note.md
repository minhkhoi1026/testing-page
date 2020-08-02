---
layout: post
title: Single Element in a Sorted Array
subtitle: An interesting binary search problem
tags: [Leetcode, Algorithm, Binary Search, Programming, Medium]
---

[Link to problem](https://leetcode.com/problems/single-element-in-a-sorted-array/)

Though this problem seem easy when using naive O(n) algorithm where you can iterate through an array and find out what element is single, it takes a little bit time to explore properties of the problem for the O(logn) algorithm.

- **Comment 1**: Since an array is sorted, two equal element always adjacent to each other

![Single Element in a Sorted Array Example](/img/2020-05-13-leetcode-may-challenge-week-2-note-1.jpg "Example Visualization")

- **Comment 2**: Let call $p$ is position of single number, then by comment 1 all $p - 1$ element before $p$ form $\dfrac{p - 1}{2}$ pair equal numbers of the form (even_postion, odd_postion).

    **Example**: {0, 0, 1, 1, 2, 4, 4, 9, 9}

    Here single element is $2$ and $p = 4$, pairs of postion before $p$ are: (0,1) and (2,3)

- **Comment 3**: Analogously, all $n - p$ element after $p$ form $\dfrac{p - 1}{2}$ pair equal numbers of the form (odd_postion, even_postion).

    **Example**: The above list have pairs of postion after $p$ are: (5,6) and (7,8)

By these comments we can build the solution using binary search:
```
left = start, right = end
while (left < right) {
    middle = (left + right) / 2
    if middle belonged to a pair of the form (even_postion, odd_postion)
        reduce the search range to (middle + 1, right)
    else
        reduce the search range to (left, middle)
}
return nums[left]
```
Detail code:
```cpp
int singleNonDuplicate(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            if ((mid % 2 == 1 &&  nums[mid] == nums[mid - 1]) 
                || (mid % 2 == 0 &&  nums[mid] == nums[mid + 1]))
                left = mid + 1;
            else
                right = mid;
        }
        return nums[left];
    }
```
Note that you can optimize a little bit by using bitwise operators, for instance: `mid = (left + right) / 2` as `mid = (left + right) >> 1`, `(mid % 2 == 1)` as `(mid & 1 == 1)`, etc.
