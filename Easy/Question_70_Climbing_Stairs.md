# Climbing Stairs
難度: Easy (javascript&Python)

題目: 爬樓梯，每次可以爬一步或兩步，有n階樓梯，有多少方法可以爬到?

限制: 1 <= n <= 45

解法:

這邊一樣可以先試看看12345階樓梯，會是怎樣的變化

```
n = 1
    climb = 1: 1
n = 2
    climb = 2: 1+1
               2
n = 3
    climb = 3: 1+1+1
               1+2
               2+1
n = 4
    climb = 5: 1+1+1+1
               1+1+2
               1+2+1
               2+1+1
               2+2
n = 5
    climb = 8: 1+1+1+1+1
               1+1+1+2
               1+1+2+1
               1+2+1+1
               2+1+1+1
               2+2+1
               2+1+2
               1+2+2
```

可以看到每一階的方法數，都是前一階方法數+前前一階方法數，跟Fibonacci的方式其實是一樣的，所以解法其實也是動態規劃與記憶法，還有遞迴

但這題使用遞迴，會有超時的問題，所以這邊就只說動態規劃

javascript
```
var climbStairs = function(n) {
    if (n<=2) return n;
    let n1=2, n2=1, result=0;
    for (let i=3;i<=n;i++) {
        result = n1+n2;
        n2 = n1;
        n1 = result;
    }
    return result;
};
```
因為限制是最小1階，而且1階2階都是直接的答案，可以直接回傳

這邊使用跟Fibonacci用array儲存的方法不同
- 使用n2代表n-2的值，n1代表n-1的值
- result表示這階的答案(n-1)+(n-2)

python3
```
class Solution:
    def climbStairs(self, n: int) -> int:
        if n<=2:
            return n
        n2 = 1
        n1 = 2
        result = 0
        for _ in range(3, n+1):
            result = n1+n2
            n2 = n1
            n1 = result
        return result
```