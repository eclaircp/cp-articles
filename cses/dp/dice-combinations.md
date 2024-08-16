# Dice Combinations

[Problem Link](https://cses.fi/problemset/task/1633)

## Gist of Question
We need to find the number of ways of getting a sum n (given integer), using the natural numbers 1 to 6.

## Explanation
We'll use Dynamic Programming to solve this problem.
<br><br>
Let dp[i] = number of ways of getting the sum i using the natural numbers 1 to 6.  
Observe one thing, dp[0] = 1 as the number of ways of getting a sum 0 is 1, i.e., by taking no number from 1 to 6.
<br><br>
We can cosider i to be the sum of a series of natural numbers consisting of only numbers from 1 to 6.  
Then, dp[i] = number of ways of getting sum i with last number in series as 1 + ... + number of ways of getting sum i with last number in series as 6
<br><br>
Therefore,
dp[i] = sum of all dp[i-j], such that, 1<=j<=6 and i-j>=0.
<br><br>
The final answer will be dp[n].

## Code

```cpp
// memoization

#include <bits/stdc++.h>
using namespace std;

const int mod = 1e9+7;
 
int f(int sum, vector<int> &dp) {
	
	if(sum==0)
		return 1;
	
	if(dp[sum] != -1)
		return dp[sum];
	
	int count = 0;
	
	for(int i=1; i<=6; i++) {
		if(sum-i>=0) {
			count = (count+f(sum-i, dp))%mod;
		}
	}
	
	return dp[sum] = count;
}
 
int main() {
	
	int n;
	cin >> n;
	
	vector<int> dp(n+1, -1);
	
	cout << f(n, dp);	
		
	return 0;
}
```

```cpp
// tabulation

#include <bits/stdc++.h>
using namespace std;

const int mod = 1e9+7;
 
int main() {
	
	int n;
	cin >> n;
	
	vector<int> dp(n+1, 0);
	
	dp[0] = 1;
	
	for(int i=0; i<n; i++) {
		
		for(int j=1; j<=6; j++) {
			 
			 if(i+j<=n) {
			 	dp[i+j] = (dp[i+j] + dp[i])%mod;
			 }
			 
		}
		
	}
	
	cout << dp[n];	
		
	return 0;
}
```
