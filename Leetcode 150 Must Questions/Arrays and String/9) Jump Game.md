## Jump Game

```md

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

 

Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

```

#### Approach I Memo
```cpp

class Solution {
public:
    bool f(int ind, vector<int> &arr, vector<int> &dp){
        if(ind == arr.size() - 1) return true;

        if(ind >= arr.size()) return false;

        if(dp[ind] != -1) return dp[ind];

        // bool can = false;
        for(int i=1; i<= arr[ind]; i++){
            if(f(ind+i, arr, dp) == true) return dp[ind] = true;
        }
        return dp[ind] = false;
    }
    bool canJump(vector<int>& nums) {
        int n= nums.size();
        vector<int> dp(n,-1);
        return f(0, nums, dp);
    }
};

```

##### Approach - II 
```cpp

using namespace std;

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxidx = 0;
        for(int i=0;i<=nums.size()-2;i++)
        {
            maxidx = max(maxidx,(i+nums[i]));
            if(maxidx >= nums.size()-1) return true;
            if((0 == nums[i]) && (maxidx <= i)) return false;
        } 
        return true;
    }
};

```