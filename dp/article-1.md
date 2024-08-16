# Chokudai - AtCoder
[Problem Link](https://atcoder.jp/contests/abc211/tasks/abc211_c)

## Gist of Question
We need to find the number of subsequences of a given string s, consisting of only lowercase English alphabets such that, the substring reads "chokudai" from left to right.

## Explanation
We'll use Dynamic Programming to solve this slve this problem.
<br><br>
Let t = "chokudai"
<br><br>
Consider dp[i][j] to be the number of subsequences of s from index 0 to i such that it is equal to the substring t<sub>0</sub>...t<sub>j</sub>
<br><br>
Observe one thing, dp[i][j] = 0, if i<j (why?)
<br><br>
dp[i-1][j] = number of subsequences of s from index 0 to i-1 such that it is equa to the substring t<sub>0</sub>...t<sub>j</sub>  
dp[i-1][j-1] = number of subsequences of s from index 0 to i-1 such that it is equa to the substring t<sub>0</sub>...t<sub>j-1</sub>
<br><br>
Therefore,  
dp[i][j] = 0, if i<j  
dp[i][j] = dp[i-1][j], if s<sub>i</sub> != t<sub>j</sub>  
dp[i][j] = dp[i-1][j] + dp[i-1][j-1], if s<sub>i</sub> != t<sub>j</sub>

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	
    string s;
    cin >> s;
    
    string t = "chokudai";
    
    int n = s.size(), m = t.size();
    
    const int mod = 1e9+7;
    
    vector<vector<int>> dp(n, vector<int>(m));
    
    dp[0][0] = ((s[0]==t[0]) ? 1 : 0);
    
    for(int i=1; i<n; i++) {
        dp[i][0] = dp[i-1][0];
        
        if(s[i]==t[0])
            dp[i][0]++;
    }
    
    for(int j=1; j<m; j++)
        dp[0][j] = 0;
    
    for(int i=1; i<n; i++) {
        for(int j=1; j<m; j++) {
            if(j>i) {
                dp[i][j] = 0;
            } else {
                dp[i][j] = dp[i-1][j]%mod;
                
                if(s[i]==t[j])
                    dp[i][j] = (dp[i][j] + dp[i-1][j-1])%mod;
            }
        }
    }
    
    cout << dp[n-1][m-1]%mod;
	
	return 0;
}
```
