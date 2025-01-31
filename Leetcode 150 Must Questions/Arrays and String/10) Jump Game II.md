## Jump Game II



You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

 

Example 1:

Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.




### Memo
```cpp

#include<bits/stdc++.h>
#include<algorithm>

using namespace std;

class Solution {
public:
    int min(int a, int b){
        if(a<b) return a;
        return b;
    }
    int f(int ind, vector<int> &nums, vector<int> &dp){
        if(ind == nums.size() - 1) {
            return 1;
        }

        if(dp[ind] != -1){
            return dp[ind];
        }

        int mini=1e5;
        int ub = min(nums.size(),nums[ind]+ind+1);
        for(int i=ind+1; i< ub;i++){
            
            int val = 1+f(i, nums,dp);
            mini = min(mini, val);
            
        }

        return dp[ind] = mini;
    }
    int jump(vector<int>& nums) {
        int n=nums.size();
        vector<int> dp(n,-1);
        return f(0, nums, dp)-1;
    }
};

```