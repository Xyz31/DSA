## 2963. Count the Number of Good Partitions

Hard

Hint
You are given a 0-indexed array nums consisting of positive integers.

A partition of an array into one or more contiguous subarrays is called good if no two subarrays contain the same number.

Return the total number of good partitions of nums.

Since the answer may be large, return it modulo 109 + 7.

 

Example 1:

Input: nums = [1,2,3,4]
Output: 8
Explanation: The 8 possible good partitions are: ([1], [2], [3], [4]), ([1], [2], [3,4]), ([1], [2,3], [4]), ([1], [2,3,4]), ([1,2], [3], [4]), ([1,2], [3,4]), ([1,2,3], [4]), and ([1,2,3,4]).
Example 2:

Input: nums = [1,1,1,1]
Output: 1
Explanation: The only possible good partition is: ([1,1,1,1]).
Example 3:

Input: nums = [1,2,1,3]
Output: 2
Explanation: The 2 possible good partitions are: ([1,2,1], [3]) and ([1,2,1,3]).
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109


```cpp

class Solution {
public:
    int numberOfGoodPartitions(std::vector<int>& nums) {
        const int MOD = 1e9 + 7;
        int n = nums.size();

        std::vector<int> dp(n, 0);
        std::unordered_map<int, int> lastindex;
        for(int i=0;i<n;i++){
            lastindex[nums[i]] = i;
        }
        
        int ans=1;
        // Two Pointers Approach
        int i=0, j=lastindex[nums[i]];
        while(i<n){
            if(i>j){
                ans = (ans * 2)%MOD;
            }
            j=max(j, lastindex[nums[i]]);
            i++;
        }

        return ans;
    }
};


```