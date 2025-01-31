## Best Time to Buy and Sell Stock II

```md

You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

```

```cpp

class Solution {
public:
    int f(int ind, int canBuy, vector<int> &arr, vector<vector<int>> &dp){
        if(ind >= arr.size()) return 0;

        if(dp[ind][canBuy] != -1) return dp[ind][canBuy];

        int take, notTake;
        if(canBuy == 1){
            take = -arr[ind] + f(ind+1, 0, arr, dp);
            notTake = f(ind+1, 1, arr, dp);
        }
        else{
            take = arr[ind] + f(ind+1, 1, arr, dp);
            notTake = f(ind+1, 0, arr, dp);
        }

        return dp[ind][canBuy] = max(take, notTake);
    }
    int maxProfit(vector<int>& prices) {
        // int n=prices.size();
        // vector<vector<int>> dp(n,vector<int>(2,-1));
        // return f(0, 1, prices, dp);
        
        int res = 0;
        for (int i = 0; i < size(prices) - 1; i++) {
            res += max(0, prices[i + 1] - prices[i]);
        }
        cout << "hey" << endl;
        return(res);

    }
};

```
