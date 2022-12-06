# Fibonacci Number
難度: Easy (javascript&Python)

題目: 就是費布納契序列
```
F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.
```
給n數，計算其F(n)為何

解法:

費布納契有規律，看下方的example
Example 1:
```
Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
```
Example 2:
```
Input: n = 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
```
Example 3:
```
Input: n = 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
```
Example 4:
```
Input: n = 5
Output: 5
Explanation: F(5) = F(4) + F(3) = 3 + 2 = 5.
```

可以看到解答分別會是這樣
```
index: 0, 1, 2, 3, 4, 5, 6, 7,  8,  9

       0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
```

這個規律是當前的項目是前一個項目和前前一個項目的答案的加總，就是費布納契的方法F(n) = F(n - 1) + F(n - 2)。

既然是加總之前的項目，可以使用遞迴(recursion)來處理

javascript
```
var fib = function(n) {
    if (n <= 1) return n;
    return fib(n-1)+fib(n-2);
};
```
python3
```
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        return self.fib(n-1)+self.fib(n-2)
```
但也很明顯的，使用遞迴的時間複雜度為O(nlogn)

另外可以用動態規劃(Dynamic Programming)和記憶法(memorization)來處理
javascript
```
var fib = function(n) {
    if (n <= 1) return n;
    let dp = [0,1];
    for (let i=2;i<=n;i++) {
        dp[i] = dp[i-1]+dp[i-2];
    }
    return dp[n];
};
```
python3
```
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        dp = [0,1]
        for i in range(2,n+1):
            dp.append(dp[i-1]+dp[i-2])
        return dp[n]
```