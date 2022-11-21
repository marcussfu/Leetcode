# Longest Common Prefix
難度: Easy (javascript)

題目: 找出字串陣列中每個項目都有且最長的字首字串

沒有就回傳空值

Example1:
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

Example2:
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

暴力解:
在strs這個陣列都一定會有一個值的前提下，可以先拿出index為0的值，再來就是從index為1的值開始遍尋strs裡的每個項目和index為0的值做比對，只要比到不一樣時，就截取從頭到該index為止的子字串，這個子字串即為最長符合字首。

- 因為是先遍尋strs裡的每個項目，再針對每個項目str的每個字去比對，所以會有雙重迴圈。

- 而在第一個迴圈時，就要比對目前result有沒有比之後要比對的str長度還要長，如果比較長的話，要先把result的長度截短到跟之後要比對的str長度一樣，這是為了避免strs只有兩項時會以為result就是最長的符合字首。

```
var longestCommonPrefix = function(strs) {
    let result = strs[0];
    for (let i=1;i<strs.length;i++) {
        if (result.length > strs[i].length) {
            result = result.substring(0, strs[i].length);
        }
        for (let j=0;j<strs[i].length;j++) {
            if (result[j] != strs[i][j]) {
                result = result.substring(0,j);
                break;
            }
        }
    }
    return result;
}
```

另一個解法思路，不用全部逐一比較，只要比最小長度的字串和最長長度的字串符合的字首有多少，因為題目要求每個項目都必須有符合的字首，所以用最短和最長比就可以了。

- 首先先將strs字串陣列從小到大排序
- 再用while迴圈比對最小字串和最大字串，直至不符合為止
- 取出最小字串從0到不符合的index為止的子字串，即為結果

```
var longestCommonPrefix = function(strs) {
    let size = strs.length;
    if (size == 1)
        return strs[0];
    strs.sort();
    let end = strs[0].length;
    let i = 0;
    while (i < end && strs[0][i] == strs[size-1][i])
        i++;
    let pre = strs[0].substring(0, i);
    return pre;
}
```

還有一個解法，javascript有一個方法為every，可以針對陣列中的每一項逐一使用方法去比對，這個方法就能使用for來處理，這邊逐一比對後，只要符合的，pre值就加1，直至不符合為止，最後再回傳到不符合為止的子字串。

```
var longestCommonPrefix = function(strs) {
    let pre = 0;
    for (let i=0;i<strs[0].length;i++) {
        if (strs.every(item => strs[0][i] === item[i])) {
            pre++;
        }
        else {
            break;
        }
    }
    return strs[0].substring(0, pre);
}
```