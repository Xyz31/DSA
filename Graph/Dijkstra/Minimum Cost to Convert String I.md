## Minimum Cost to Convert String I


2976. Minimum Cost to Convert String I

You are given two 0-indexed strings source and target, both of length n and consisting of lowercase English letters. You are also given two 0-indexed character arrays original and changed, and an integer array cost, where cost[i] represents the cost of changing the character original[i] to the character changed[i].

You start with the string source. In one operation, you can pick a character x from the string and change it to the character y at a cost of z if there exists any index j such that cost[j] == z, original[j] == x, and changed[j] == y.

Return the minimum cost to convert the string source to the string target using any number of operations. If it is impossible to convert source to target, return -1.

Note that there may exist indices i, j such that original[j] == original[i] and changed[j] == changed[i].

 

```md
Example 1:

Input: source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
Output: 28
Explanation: To convert the string "abcd" to string "acbe":
- Change value at index 1 from 'b' to 'c' at a cost of 5.
- Change value at index 2 from 'c' to 'e' at a cost of 1.
- Change value at index 2 from 'e' to 'b' at a cost of 2.
- Change value at index 3 from 'd' to 'e' at a cost of 20.
The total cost incurred is 5 + 1 + 2 + 20 = 28.
It can be shown that this is the minimum possible cost.
Example 2:

Input: source = "aaaa", target = "bbbb", original = ["a","c"], changed = ["c","b"], cost = [1,2]
Output: 12
Explanation: To change the character 'a' to 'b' change the character 'a' to 'c' at a cost of 1, followed by changing the character 'c' to 'b' at a cost of 2, for a total cost of 1 + 2 = 3. To change all occurrences of 'a' to 'b', a total cost of 3 * 4 = 12 is incurred.
Example 3:

Input: source = "abcd", target = "abce", original = ["a"], changed = ["e"], cost = [10000]
Output: -1
Explanation: It is impossible to convert source to target because the value at index 3 cannot be changed from 'd' to 'e'.
 

Constraints:

1 <= source.length == target.length <= 105
source, target consist of lowercase English letters.
1 <= cost.length == original.length == changed.length <= 2000
original[i], changed[i] are lowercase English letters.
1 <= cost[i] <= 106
original[i] != changed[i]

```

### Approach -I Floys Warshall Algorithm
```cpp

class Solution {
public:
    void floydWarshall(vector<vector<int>>&g){ // adjust for list form
        int n=g.size();
        for(int k=0;k<n;++k){
            for(int i=0;i<n;++i){
                for(int j=0;j<n;++j){
                    if(g[i][k]<INT_MAX && g[k][j]<INT_MAX){
                        g[i][j]=min(g[i][j], g[i][k]+g[k][j]);
                    }
                }
            }
        }
    }
    long long minimumCost(string src, string tar, vector<char>&from, vector<char>&to, vector<int>& cost) {
        int n=src.size(), m=cost.size();
        vector<vector<int>>g(26, vector<int>(26, INT_MAX));
        for(int i=0; i<m; ++i){
            int x = from[i]-'a', y=to[i]-'a';
            g[x][y] = min(g[x][y], cost[i]);
        }
        floydWarshall(g);
        long long ans=0;
        for(int i=0; i<n; ++i){
            int x = src[i]-'a', y = tar[i]-'a';
            if(x==y) continue;
            if(g[x][y]==INT_MAX) return -1;
            ans+=g[x][y];
        }
        return ans;
    }
};

```

### Approach - 2 Dijkstra Algorithm
```cpp
class Solution {
public:
    void bfs(unordered_map<char,vector<pair<char,int>>>&graph,char source,vector<vector<int>>&distances){
//Normal BFS shortest path code
//Saving calculated distance in distances array

        queue<pair<int,int>>q;
        q.push({source,0});
        while(!q.empty()){
            int node = q.front().first;
            int distance=q.front().second;
            q.pop();
            for(auto child:graph[node]){
                if(distances[source-'a'][child.first-'a']>distance+child.second){

                    distances[source-'a'][child.first-'a']=distance+child.second;
                    q.push({child.first,distance+child.second});
                }
            }
            
        }
        return;
    }
    
    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) {

        unordered_map<char,vector<pair<char,int>>>graph;

//Initialzing the graph in the node schema : {parent,{child,cost}}
        for(int i=0;i<original.size();i++){
            graph[original[i]].push_back({changed[i],cost[i]});
        }

        //Distance array required will only need 26*26 space.
        vector<vector<int>>distances(26,vector<int>(26,INT_MAX));

        //Running BFS from every node of graph.
        for(auto it:original)
        bfs(graph,it,distances);
        
        long long ans=0;
        for(int i=0;i<source.size();i++){

//No need to add anything to answer if source and target are same
            if(source[i]==target[i])continue;

//If Distance is infinite, the target is not achievable and hence return -1
            if(distances[source[i]-'a'][target[i]-'a']==INT_MAX)return -1;

//Otherwise add the corresponding value in the distances array
            else ans+=distances[source[i]-'a'][target[i]-'a'];
        }
        
        return ans;
    }
};

```