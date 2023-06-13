# Merge Strings Alternately
難度: Easy (javascript)

題目: 給兩個字串，交替合成一個字串，word1先，

如果其中一個字串先結束，就把剩下的字串繼續加上去。

解法: 

while迴圈
1. 給word1和word2各一個目前的index(從零開始)
2. 然後while迴圈判斷只要任一index還在字串範圍內，就繼續執行
3. 在while迴圈裡依序先把word1的字元加到margeStr，再加word2的字元
4. 然後累加word1和word2的目前index，直到查找自己的字串完成為止
5. 回傳margeStr

for迴圈
1. 先用Math的max函數去找出word1和word2哪個長度最長，以此值來開始for迴圈
2. 在for迴圈裡，一樣交互把word1和word2的字元加到margeStr裡
3. 如果中間其中哪個字串判斷已經沒有字元(null)，那就|| ''，加上空白值
4. 因為用最長的字串當for迴圈了，所以不用累加目前index
5. 回傳margeStr

javascript(While)
```
var mergeAlternately = function(word1, word2) {
    let margeStr = '', word1Curr = 0, word2Curr = 0;
    while (word1Curr <= word1.length - 1 || word2Curr <= word2.length -1) {
        margeStr += word1Curr <= word1.length - 1? word1[word1Curr]: '';
        margeStr += word2Curr <= word2.length - 1? word2[word1Curr]: '';
        word1Curr++;
        word2Curr++;
    }
    return margeStr;
};
```

javascript(for)
```
var mergeAlternately = function(word1, word2) {
    const n = Math.max(word1.length, word2.length);
    let margeStr = '';
    
    for (let i=0;i<n;i++) {
        margeStr += word1[i] || '';
        margeStr += word2[i] || '';
    }
    return margeStr;
};
```