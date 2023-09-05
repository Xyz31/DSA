## Given an array of positive integers arr, return the sum of all possible odd-length subarrays of arr.

```md

A subarray is a contiguous subsequence of the array.

Example 1:

Input: arr = [1,4,2,5,3]
Output: 58
Explanation: The odd-length subarrays of arr and their sums are:
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
If we add all these together we get 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58

```

#### Brute 
```cpp

class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int ans = 0;
        int n = arr.size();
        
        // Brute force

        
        // Sum of all elements in the array
        ans += std::accumulate(arr.begin(), arr.end(), 0);
        
        // Initialize prefix sum array
        std::vector<int> prefix(n);
        prefix[0] = arr[0];
        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + arr[i];
        }
        
        // Calculate sum of odd-length subarrays
        for (int length = 3; length <= n; length += 2) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                if (i == 0) {
                    ans += prefix[j]; // Sum of [0, j]
                } else {
                    ans += (prefix[j] - prefix[i - 1]); // Sum of [i, j]
                }
            }
        }
        return ans;
        
    }
};

```

#### Optimized
```cpp

class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int ans = 0;
        int n = arr.size();
        
        // optimised code

        // key observations
        
        /* 1) The number of subarrays including the element at the ith index is always equal to the number of subarrays including the element at the index N – i. Therefore, simultaneously update the count for both the indices.

        2) To calculate the number of subarrays that include the element at the ith index, we simply subtract the number of subarrays not including the element at the ith index from the total number of ways.
        */
        for (int i = 0; i < n; i++) {
            // Approach-1

            // int leftCount = i;
            // int rightCount = n - i;
            // int totalPairs = (leftCount + 1) * (rightCount);
            // int oddpairs = ceil(totalPairs/2.0);
            // ans += (oddpairs*arr[i]);

            // Approach-2
    
            // x = Total number of subarrays - total number of subarrays which do not include the element
            int x = n*(i+1) - i*(i+1);
            int odd = ceil(x/2.0);
            ans += (odd*arr[i]);
            
        }

        return ans;
        
    }
};

```