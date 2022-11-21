# Roman to Integer
難度: Easy (javascript)

題目: 把羅馬字轉為數字
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
- I can be placed before V (5) and X (10) to make 4 and 9. 
- X can be placed before L (50) and C (100) to make 40 and 90. 
- C can be placed before D (500) and M (1000) to make 400 and 900.

Example 1: 
2 -> II
12 -> XII
27 -> XXVII

Example 2: 
```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

解法:
正常的羅馬字排法是從大到小，像XII->12

我想到的是for迴圈除了最後一個，之前的每個項目都要檢查下一個項目的值有沒有比自已大，這邊會先用hasp table把羅馬字表示的數字存起來以便比對。

像是IV，如果I檢查到下一個項目V(5)比自已的I(1)大，那自已的這個I值就要乘於-1，這樣-1+5就會等於4。
```
var romanToInt = function(s) {
    const romanDigit = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000,
    };

    let result = 0;
    for (let i=0;i<s.length;i++) {
        if (i == s.length - 1) {
            result += romanDigit[s[i]];
        }
        else {
            result += romanDigit[s[i]] * (romanDigit[s[i+1]]>romanDigit[s[i]]? -1:1);
        }
    }
    return result;
}
```

提示是寫最簡單的寫法是從後面到前面檢示每個值，並用hash table來比對運算，可以想成只要目前項目的值跟後一個值比較為小，那就表示目前項目的值要為負被減掉。(自已這樣寫覺得只是上面的反過來罷了)

```
var romanToInt = function(s) {
    const romanDigit = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000,
    };

    let result = romanDigit[s[s.length-1]];
    for (let i=s.length-2;i>=0;i--) {
        result += romanDigit[s[i]] * (romanDigit[s[i]] < romanDigit[s[i+1]]? -1:1);
    }
    return result;
}
```

時間複雜度為O(n)

我想可以再優化的部分，是不要用到乘，至於正反沒差

```
var romanToInt = function(s) {
    const romanDigit = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000,
    };

    let result = 0;
    for (let i=0;i<s.length;i++) {
        if (i == s.length - 1) {
            result += romanDigit[s[i]];
        }
        else {
            romanDigit[s[i+1]]>romanDigit[s[i]]? result -= romanDigit[s[i]]: result += romanDigit[s[i]];
        }
    }
    return result;
}
```