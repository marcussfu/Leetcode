# Sqrt(x)
難度: Easy (javascript)

題目: 給一個不是負數的整數，回傳其平方根為何

Example 1:
```
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```
Example 2:
```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

限制: 
- 不能用內建的方法(pow、sqrt等)
- 0 <= x <= 231 - 1

解法: 

- 判斷如果x為0，平方根就是0
- 直接從2開始，如果x為1的話，那結果也會回傳2-1
- 從while開始遍尋，只要s*s小於傳入的x，那就表示還不算是平方，繼續迴圈
- 但只要大於，就可以離開迴圈開始判斷答案
- 如果s*s是大於，那表示前一個s還是小於，就回傳s-1
- 如果s*s是等於，那就直接回傳s為平方根

```
var mySqrt = function(x) {
    if (x == 0)
        return 0;
    s = 2;
    while(s*s < x) {
        s++;
    }
    return s*s >x? s-1:s;
}
```