# Partition DP

## Format
A linear data structure will be given and we would have to divide it into multiple parts to get some condition satisfied. We would take a segment [i.....j] such that, i<=j and divide this segment into parts like [i.....k] and [k....j] such that, i<=k<=j, i.e., k would vary between i and j and we would solve the problem for those sub-segments and use the temporary answer got from it to calculate the answer for the segment [i.....j] and hence for the complete length of the given linear data structure.

## Questions

### Matrix Chain Multiplication

#### Gist of Question
Given an array of the dimensions of a series of matrices, which can be multiplied in the sequence in which they are given, we need to minimize the number of operations required to get the product of the matrices.

#### Approach
Let the given array be arr[], then the dimensions of the i<sup>th</sup> matrix is arr[i-1] x arr[i], where i>=1.
<br><br>
We'll define a function f(i, j), which will return the minimum number of operations required to for the matrices i to j.  
This can be divided into sub-problems f(i, k) and f(k+1, j), where i<=k<j and the base case being i=j, whenin we return 0, as no operation is required for a sigle matrix.  
The new matrices formed after diving i.....j into i...k and k+1....j will have dimensions arr[i-1] x arr[k] and arr[k] x arr[j]. The number of operations required to multiply these two newly formed matrices is arr[i-1] x arr[k] x arr[j].
<br><br>
DP State: dp[i][j] = minimum number of operations required for the matrices i.....j  
Transtition: dp[i][j] = min (dp[i][k] + dp[k+1][j] + arr[i-1]*arr[k]*arr[j]), i<=k<j

#### Code

```cpp
// memoization

class Solution{
    
    vector<vector<int>> dp;
    
    int f(int i, int j, int arr[]) {
        
        if(i==j) {
            return 0;
        }
        
        if(dp[i][j] != -1) {
            return dp[i][j];
        }
        
        int ans = INT_MAX;
        
        for(int k=i; k<j; k++) {
            ans = min(ans, f(i, k, arr) + f(k+1, j, arr) + arr[i-1]*arr[k]*arr[j]);
        }
        
        return dp[i][j] = ans;
        
    }
    
public:

    int matrixMultiplication(int n, int arr[]) {
        
        dp.resize(n, vector<int> (n, -1));
        
        return f(1, n-1, arr);
        
    }
};
```

```cpp
// tabulation

/*
we first calculate the min number of operations required for segment of matrices of size 1,
which will all be 0 (base case)
then, we calculate the answers for segment of matrices from 2 to n-1 matrices
*/

class Solution{
    
    vector<vector<int>> dp;
    
public:

    int matrixMultiplication(int n, int arr[]) {
        
        dp.resize(n, vector<int> (n));
        
        for(int i=1; i<n; i++)
            dp[i][i] = 0;
            
        for(int len=2; len<=n-1; len++) {
            for(int i=1; i<n-len+1; i++) {
                
                int j = i+len-1;
                
                dp[i][j] = INT_MAX;
                
                for(int k=i; k<j; k++) {
                    dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + arr[i-1]*arr[k]*arr[j]);
                }
            }
        }
        
        return dp[1][n-1];
        
    }
};
```

### Minimum Cost to Cut a Stick

#### Gist of Question
Given the length of a stick and an array of integers representing the positions where to make cut, we need to minimize the cost of cutting the stick at all those positions such that the cost of making a cut at a particular position is equal to the length of the sub-stick one is making a cut on.

#### Code

```cpp
// memoization

class Solution {

    int m;
    vector<vector<int>> dp;
    vector<int> arr;
 
    int f(int i, int j) {

        if(j-i<=1) {
            return 0;
        }

        if(dp[i][j] != -1) {
            return dp[i][j];
        }

        int ans = INT_MAX;

        for(int k=i+1; k<j; k++) {
            ans = min(ans, arr[j] - arr[i] + f(i, k) + f(k, j));
        }

        if(ans == INT_MAX) {
            ans = 0;
        }

        return dp[i][j] = ans;

    }

public:
    int minCost(int n, vector<int>& cuts) {

        arr.push_back(0);

        for(auto ele : cuts) {
            arr.push_back(ele);
        }

        arr.push_back(n);

        sort(arr.begin(), arr.end());

        m = arr.size();

        dp.resize(m, vector<int> (m, -1));

        return f(0, m-1);
    }
};
```
