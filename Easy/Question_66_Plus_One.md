# Plus One
難度: Easy (javascript)

題目: 給一個integer陣列加1，回傳加1後的結果，要考慮到進位的問題。

Example 1:
```
Input: digits = [1,2,3]
Output: [1,2,4]
```
Example 2:
```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
```
Example 3:
```
Input: digits = [9]
Output: [1,0]
```

解法:

這題可以想兩個解法
- 把陣列先轉成數字加1後，再轉回陣列
- 遍尋每個項目，然後根據目前項目加減數值

這邊選擇用遍尋每個項目的方法，因為這個方法不用再另外產生陣列，可以直接在原陣列做操作。

- 先設置carry代表這次遍尋的項目值是否需要進位
- 倒序遍尋陣列，這樣才能從個位數開始增加
- 第一個if判斷是否是個位數或有進位數，因為如果是個位數和有進位數，表示我們需要在當前項目加1，因為已經加1，這邊把carry判斷進位數設為false。
- 第二個if判斷當前項目經過第一個判斷後，是否有超過9(已經為10)需要"再進位"，如果需要的話，那當前的項目就設定為0，因為要進位，把carry判斷進位數設為true。
- 最後一個if判斷如果項目已經是最後(最前面)了，但carry判斷進位數還是true，表示需要在陣列最前面增加一個數字1的項目。

```
var plusOne = function(digits) {
    let carry = false;
    for (let i=digits.length-1;i>=0;i--) {
        if (i == digits.length-1 || carry) {
            digits[i] += 1;
            carry = false;
        }
        if (digits[i] > 9) {
            digits[i] = 0;
            carry = true;
        }
        if (i == 0 && carry) {
            digits.unshift(1);
        }
    }
    return digits;
};
```

別人的解法:

一樣是用倒序遍尋的，但更精簡
- 每次遍尋項目都加1，這樣的做法是如果一直需要進位，就直接把目前項目加1。
- 判斷如果這次項目超過9需要再進位，就把目前項目設定為0，讓遍尋繼續進位下去。
- 如果這次的項目不超過9，那就表示進位已經結束，可直接回傳答案，不用再遍尋下去。
- 最後如果整個遍尋結束，仍未回傳答案，那表示最後一個項目需要再進位，這邊就會直接在陣列最前面增加一個數字1的項目。

```
var plusOne = function(digits) {
    for (let i = digits.length-1;i >=0;i--) {
        digits[i]++;
        if (digits[i] > 9) {
            digits[i] = 0;
        }
        else {
            return digits;
        }
    }
    digits.unshift(1);
    return digits;
}
```

