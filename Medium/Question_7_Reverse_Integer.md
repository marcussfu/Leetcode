# Reverse Integer
難度: Medium(javascript&python3)

題目: 拿到一個32位元的數字，把它反過來後回傳。

如果反過來後數字超過了32位元的範圍，回傳0。

Example 1:
```
Input: x = 123
Output: 321
```
Example 2:
```
Input: x = -123
Output: -321
```
Example 3:
```
Input: x = 120
Output: 21
```

限制: -231 <= x <= 231 - 1

解法: 

- 判斷傳入的數值x是不是負數，先把值變成正數
- 這邊用while，從個位數開始把值存到rev裡
- 這邊用10去運算，求出每次x的餘數和除數
- 餘數就是每次運算要"往前"推的值
- 除數則是讓x從個、十到百這樣推進的值
- rev也是跟著每個迴圈從個十到百這樣乘，再加上每次要往前推進的餘數
- 在算完整個反轉數後，記得一開始有先記下這個數值是負數否，把這個值乘成負數(如果是負數的話)
- 最後判斷有沒有超過32位元的範圍，有的話就回傳0

python3
```
class Solution:
    def reverse(self, x: int) -> int:
        neg = False
        if x <0:
            neg=True
            x = x * -1
            
        rev =0
        while x:
            n = x %10
            x = x // 10
            rev = rev * 10 + n
            
        if neg:
            rev = rev* -1
            
        mi = 2 ** 31 * (-1)
        ma = 2 ** 31 - 1
        if rev < mi or rev > ma:
            return 0
        return rev
```

javascript
```
var reverse = function(x) {
    let neg = x < 0? -1:1;
    x *= neg;

    let rev=0, n=0;
    while (x) {
        n = x % 10;
        x = Math.trunc(x/10);
        rev = rev * 10 + n;
    }

    rev *= neg;

    let mi = 2 ** 31 * -1, ma = 2 ** 31 -1;
    if (rev < mi || rev > ma)
        return 0;
    return rev;
};
```