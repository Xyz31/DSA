## Remove Duplicates from Sorted Array

```md

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

```

```cpp

class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //Day-7 (LinkedList and arrays) Q-5;
        //Brute use hashset T-O(NlogN + N) , S-O(N)
        //Optimal use Two pointers method T-O(N) , S(N)
        int res=0;
        int i=0;
        
        for(int k =i+1;k<nums.size();k++){
            if(nums[k] != nums[i]){
                i++;
                nums[i]=nums[k];
            }
        }
        return i+1;
    }
};

```