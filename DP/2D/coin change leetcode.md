## Coin Change

```md

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0

```

#### code

##### MEMOIZATION

```cpp

class Solution {
public:
    int dp[13][10001];

    // Recursive function to find the minimum number of coins needed
    // to make up the 'amount' using the first 'n' coin denominations.
    int f(vector<int>& coins, int amount, int n)
    {
        // Base case 1: If 'n' or 'amount' is non-positive, it's impossible to make up the amount.
        if (n <= 0 || amount <= 0)
            return (amount == 0) ? 0 : INT_MAX - 1;

        // If the result for the current 'n' and 'amount' is already calculated, return it.
        if (dp[n][amount] != -1)
            return dp[n][amount];

        // Initialize variables to store the results of "not picking" and "picking" the 'n'-th coin.
        int notpick = f(coins, amount, n - 1);
        int pick = INT_MAX;

        // Check if it's possible to pick the 'n'-th coin denomination without exceeding the 'amount'.
        if (coins[n - 1] <= amount)
            pick = 1 + f(coins, amount - coins[n - 1], n);

        // Store the minimum of "not picking" and "picking" in 'dp[n][amount]'.
        return dp[n][amount] = min(pick, notpick);
    }

    // Function to calculate the minimum number of coins needed to make up the 'amount'.
    int coinChange(vector<int>& coins, int amount) {
        memset(dp, -1, sizeof(dp)); // Initialize 'dp' with -1 (not computed) values.
        int n = coins.size(); // Number of coin denominations available.
        int ans = f(coins, amount, n); // Call the recursive function to find the result.
        
        // If the result is 'INT_MAX-1', it means it's not possible to make up the amount.
        // Return -1 in this case, otherwise, return the minimum number of coins needed.
        return (ans == INT_MAX - 1) ? -1 : ans;
    }
};


```


##### TABULATION
```cpp



```