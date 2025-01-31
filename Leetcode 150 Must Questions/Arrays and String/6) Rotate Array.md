## Rotate Array

```md

Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

 

Example 1:

Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

```

```cpp

class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n=nums.size();
        k = k%n;
        int front = n-k;
        reverse(nums.begin(), nums.begin()+front);
        // for(int x: nums) cout<<x<<" ";
        reverse(nums.begin()+front, nums.end());
        reverse(nums.begin(), nums.end());
        return;
    }
};

```