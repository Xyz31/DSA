## 3425. Longest Special Path
Solved
Hard

```md 
You are given an undirected tree rooted at node 0 with n nodes numbered from 0 to n - 1, represented by a 2D array edges of length n - 1, where edges[i] = [ui, vi, lengthi] indicates an edge between nodes ui and vi with length lengthi. You are also given an integer array nums, where nums[i] represents the value at node i.

A special path is defined as a downward path from an ancestor node to a descendant node such that all the values of the nodes in that path are unique.

Note that a path may start and end at the same node.

Return an array result of size 2, where result[0] is the length of the longest special path, and result[1] is the minimum number of nodes in all possible longest special paths.

 

Example 1:

Input: edges = [[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], nums = [2,1,2,1,3,1]

Output: [6,2]

Explanation:

In the image below, nodes are colored by their corresponding values in nums


The longest special paths are 2 -> 5 and 0 -> 1 -> 4, both having a length of 6. The minimum number of nodes across all longest special paths is 2.

Example 2:

Input: edges = [[1,0,8]], nums = [2,2]

Output: [0,1]

Explanation:



The longest special paths are 0 and 1, both having a length of 0. The minimum number of nodes across all longest special paths is 1.

 

Constraints:

2 <= n <= 5 * 10^4
edges.length == n - 1
edges[i].length == 3
0 <= ui, vi < n
1 <= lengthi <= 10^3
nums.length == n
0 <= nums[i] <= 5 * 10^4
The input is generated such that edges represents a valid tree.

```

### Using DP Extra : S-O(N)

```cpp


class Solution {
public:
    vector<vector<pair<int,int>>> adj;
    unordered_map<int , int> vis; // to check whether a color is already added to path
    vector<int> path; // to store nodes in current path
    vector<int> prefix; // to store cumulative length
    vector<pair<int , int>> dp; // to store max length , count for each valid path considering i-th node as descendent

    void dfs(int node , int par , int topnodeIndex , int curlen  , vector<int> &colors) 
    {
        // get top node of the current path
        int topnode = path[topnodeIndex];
        // get total length from top node to current node of current path
        int len = curlen - prefix[topnodeIndex];
        // get total no of nodes from top node to current node of current path
        int countnode = path.size() - path[topnodeIndex];

        if(dp[node].first < len) {
            dp[node].first = len;
            dp[node].second = countnode;
        } else if(dp[node].first == len) {
            dp[node].second = min(dp[node].second , countnode);
        }

        int index = path.size();

        // apply dfs
        for(auto [v , wt] : adj[node]) {
            if(v == par) continue;

            int newlen = curlen + wt;
            path.push_back(v);
            int col = colors[v];

            // if same color not taken before
            if(vis.find(col) == vis.end()) {
                vis[col] = index;
                dfs(v , node , topnodeIndex , newlen , colors);
                vis.erase(col);
            }
            else{
                int prevIndex = vis[col];
                vis[col] = index;
                dfs(v  , node , max(topnodeIndex , prevIndex+1) , newlen , colors);
                vis[col] = prevIndex;
            }
        }
        path.pop_back();
    }
    vector<int> longestSpecialPath(vector<vector<int>>& edges, vector<int>& nums) {
        int n = nums.size();
        adj.resize(n+1);
        dp.resize(n+1);

        // Build the adjacency list
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1], length = edge[2];
            adj[u].push_back({v, length});
            adj[v].push_back({u, length});
        }

        
        dfs(0, -1, 0, 0, nums); // Start DFS from the root (node 0)
        vector<int> result(2);
        for(auto [u , wt] : dp) {
            if(result[0] < wt) {
                result[0] = wt;
                result[1] = u;
            }else if(result[0] == wt) result[1] = min(result[1] , u);
        }
        return result;
    }
};


```

### Using Constant Extra Space - O(1)
```cpp

class Solution {
public:
    vector<vector<pair<int, int>>> adj; // adjacency list
    unordered_map<int, int> vis;       // track if a color is already in the path
    vector<int> path;                  // current path of nodes
    vector<int> prefix;                // cumulative length along the path
    vector<pair<int, int>> dp;         // max length and min nodes for each valid path

    void dfs(int node, int parent, int topnodeIndex, int curlen, vector<int>& colors) {
        // Top node of the current path
        int topnode = path[topnodeIndex];
        // Length of the current path
        int len = curlen - prefix[topnodeIndex];
        // Number of nodes in the current path
        int countnode = path.size() - topnodeIndex;

        // Update dp[node]
        if (dp[node].first < len) {
            dp[node] = {len, countnode};
        } else if (dp[node].first == len) {
            dp[node].second = min(dp[node].second, countnode);
        }

        int index = path.size(); // Current path index

        // Traverse neighbors
        for (auto [v, wt] : adj[node]) {
            if (v == parent) continue; // Skip parent

            int newlen = curlen + wt;
            path.push_back(v);        // Add to path
            prefix.push_back(newlen); // Add to prefix sum
            int col = colors[v];

            // If the color is not in the path
            if (vis.find(col) == vis.end()) {
                vis[col] = index;
                dfs(v, node, topnodeIndex, newlen, colors);
                vis.erase(col);
            } else {
                // If the color is already in the path
                int prevIndex = vis[col];
                vis[col] = index;
                dfs(v, node, max(topnodeIndex, prevIndex + 1), newlen, colors);
                vis[col] = prevIndex;
            }

            path.pop_back();   // Backtrack path
            prefix.pop_back(); // Backtrack prefix
        }
    }

    vector<int> longestSpecialPath(vector<vector<int>>& edges, vector<int>& nums) {
        int n = nums.size();
        adj.resize(n);
        dp.assign(n, {0, INT_MAX}); // Initialize dp with {0, INT_MAX}

        // Build the adjacency list
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1], length = edge[2];
            adj[u].push_back({v, length});
            adj[v].push_back({u, length});
        }

        path.push_back(0);       // Start with root node
        prefix.push_back(0);     // Initial cumulative length
        vis[nums[0]] = 0;        // Mark root node's color
        dfs(0, -1, 0, 0, nums); // Start DFS

        // Find the result
        vector<int> result = {0, INT_MAX};
        for (auto& [len, count] : dp) {
            if (result[0] < len) {
                result = {len, count};
            } else if (result[0] == len) {
                result[1] = min(result[1], count);
            }
        }

        return result;
    }
};


```