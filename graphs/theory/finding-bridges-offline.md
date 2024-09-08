# Finding Bridges Offline
A bridge is an edge which, when removed, makes the graph disconnected (increases the number of connected components in the graph).

## Algorithm
For any vertex u, if we run a DFS from it, any edge (u, v) is a bridge iff none of the vertices v and its descendents in the DFS traversal has a back-edge to u or any of its ancestors.
<br><br>
Let t_in[] store new names for nodes depending upon the time at which it's visited in a DFS traversal. This gives chronological names to the nodes.
<br><br>
Let low[] store the node with the smallest new name that is connected to the current node.
<br><br>
For all edges (u, v), if low[v] > t_in[u], (u, v) is a bridge.
<br><br>
```cpp
class Solution {

    int t = 0;
    vector<int> t_in, low;
    vector<vector<int>> adj;
    vector<bool> vis;

    void dfs(int node, int parent) {

        t++;
        t_in[node] = t;
        low[node] = t;
        vis[node] = true;

        for(auto adjNode : adj[node]) {
            if(!vis[adjNode]) {
                dfs(adjNode, node);
                low[node] = min(low[node], low[adjNode]);
            } else if(adjNode != parent) {
                low[node] = min(low[node], low[adjNode]);
            }
        }

    }

public:

    vector<vector<int>> getBridges(int n, vector<vector<int>>& edges) {
        
        adj.resize(n);
        t_in.resize(n);
        low.resize(n);
        vis.resize(n, false);

        for(auto v : edges) {
            adj[v[0]].push_back(v[1]);
            adj[v[1]].push_back(v[0]);
        }

        for(int node=0; node<n; node++) {
            if(!vis[node]) {
                dfs(node, 0);
            }
        }

        vector<vector<int>> bridges;

        for(auto v : edges) {
            if(low[v[1]] > t_in[v[0]] || low[v[0]] > t_in[v[1]]) {
                bridges.push_back(v);
            }
        }

        return bridges;
 
    }
};
```
