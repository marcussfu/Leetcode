# Longest Substring Without Repeating Characters
難度: Medium (javascript)

題目: 從給的字串去取得最長的不重覆子字串有多長。

Example 1:
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc".
```
Example 2:
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b".
```
Example 3:
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke".
```
Example 4:
```
Input: s = "dvdf"
Output: 3
Explanation: The answer is "vdf".
```
Example 5:
```
Input: s = " "
Output: 1
Explanation: The answer is " ".
```

限制:
- 0 <= s.length <= 5 * 104
- 字串包含英文字、數字、符號或空白

解法: 

brute force and sliding window的解法

暴力解就是遍尋s字串的每個字，然後從每個字再去遍尋這個字到字串尾沒有重覆的長度，把每次的長長度比較，留下最長的，即是答案。

但這樣的時間複雜度就是O(n*m)。

```
var lengthOfLongestSubstring = function(s) {
    let result = 0;
    for (let i=0;i<s.length;i++) {
        let dic = {};
        let count = 0;
        for (let j=i;j<s.length;j++) {
            if (s[j] in dic) {
                break;
            }
            else {
                dic[s[j]] = s[j];
            }
            count++;
        }
        result = count>result? count:result;
    }
    return result;
};
```

sliding window

這個解法跟two pointer是很類似的，但我目前還沒有很懂，只是知道一樣是設置開始和結尾的index，隨著推進結尾的index，在滿足某些條件下，會讓開始的index也跟著移動。

這樣的題目通常是求字串中不重覆的最長字串，或陣列內項目相加的最大數之類。

這題的想法是
- 先設置開始的wStart和結尾會跟著目前狀態跑的wEnd
- 也設置了比較後最長的不重覆長度值的result
- 這邊用Set這個結構來儲存整個不重覆字元
- 用while迴圈推進wEnd到整個字串的結尾，每次都去判斷目前dic有沒有重覆的值，沒有就存入
- 因為新增了一筆不重覆的值，所以需要比較目前result的值和目前的wEnd-wStart+1(因為0也算1筆)那個不重覆的長度比較長，把最長的存回result
- 確定存入不重覆值後，增加wEnd去檢查下一個值有沒有重覆
- 如果遍尋到目前wEnd在dic中有重覆的值，這表示我們已經在result裡存入了到目前wEnd為止的最長不重覆值長度了。
- 為了要檢查下一段不重覆值子字串的長度，需要把已經存入dic的wEnd值刪掉，以便重新加入新的不重覆值子字串，但問題來了，為什麼卻是刪掉dic的wStart值?
- 這邊有一句我自己覺得的解釋，"把重覆值之前的字串，刪到把重覆值從dic刪掉為止"
- 因為目前wEnd的值已經在dic有重覆了，所以從wStart把值都在dic刪掉，並推進wStart，刪到把重覆的值從dic刪掉
- 刪掉重覆值的過程，wEnd會一直卡在重覆值的index，直到刪掉為止
- 刪掉重覆值後，就可以在dic存入這個已經不重覆的值，繼續推進查看不重覆的最長子字串長度

這邊有兩個example的解釋

example1:

```
s = "dvdf"
s[wEnd]:     d, v,  d,  d,  f
dic:         d, dv, v,  vd, vdf
result:      1, 2,  2,  2,  3
++後的wEnd:   1, 2,  2,  3,  4
++後的wStart: 0, 0,  1,  1,  1
```

example2:

```
s = "vddf"
s[wEnd]:     v, d,  d,  d,    d,  f
dic:         v, vd, d, empty, d,  df
result:      1, 2,  2,  2,    2,  2
++後的wEnd:   1, 2,  2,  2,    3,  4
++後的wStart: 0, 0,  1,  2,    2,  2
```

```
var lengthOfLongestSubstring = functiobn(s) {
    let result = 0;
    let wStart = 0;
    let wEnd = 0;
    let dic = new Set();
    while(wEnd < s.length) {
        if (!dic.has(s[wEnd])) {
            dic.add(s[wEnd]);
            result = result > wEnd-wStart+1? result:wEnd-wStart+1;
            wEnd++;
        }
        else {
            dic.delete(s[wStart]);
            wStart++;
        }
    }
    return result;
};
```