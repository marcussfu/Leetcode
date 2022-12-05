# Minimum Average Difference
難度: Medium (javascript)

題目: 給一個index從0開始的陣列，回傳最小平均差的index，這題其實我沒有很了解題目，是從example去推需要處理的平均差。

Example 1:
```
Input: nums = [2,5,3,9,5,3]
Output: 3
Explanation:
- The average difference of index 0 is: |2 / 1 - (5 + 3 + 9 + 5 + 3) / 5| = |2 / 1 - 25 / 5| = |2 - 5| = 3.
- The average difference of index 1 is: |(2 + 5) / 2 - (3 + 9 + 5 + 3) / 4| = |7 / 2 - 20 / 4| = |3 - 5| = 2.
- The average difference of index 2 is: |(2 + 5 + 3) / 3 - (9 + 5 + 3) / 3| = |10 / 3 - 17 / 3| = |3 - 5| = 2.
- The average difference of index 3 is: |(2 + 5 + 3 + 9) / 4 - (5 + 3) / 2| = |19 / 4 - 8 / 2| = |4 - 4| = 0.
- The average difference of index 4 is: |(2 + 5 + 3 + 9 + 5) / 5 - 3 / 1| = |24 / 5 - 3 / 1| = |4 - 3| = 1.
- The average difference of index 5 is: |(2 + 5 + 3 + 9 + 5 + 3) / 6 - 0| = |27 / 6 - 0| = |4 - 0| = 4.
The average difference of index 3 is the minimum average difference so return 3.
```
Example 2:
```
Input: nums = [0]
Output: 0
Explanation:
The only index is 0 so return 0.
The average difference of index 0 is: |0 / 1 - 0| = |0 - 0| = 0.
```

解法:

照這樣的規則，很明顯就是跟據index，來處理左邊平均-右邊平均，
- 隨著index遍尋，左邊的平均被除數會增加，左邊的除數(index+1)
- 隨著index遍尋，右邊的平均被除數會減少，右邊的除數(length-(index+1))
- 然後左邊的平均-右邊的平均，取其絕對值，不要有負數的平均差
- 最後找出最小的平均差的index，即為答案
  
這邊用了prefix sum 前綴和的概念，先把index對應的加總值都先存起來
- 先設定result為無限大值，因為我們要找最小值
- 接著就是跟據上面的左邊平均-右邊平均的方法，求出每個index的平均差
- 這邊要注意，因為javascript，0/0會為nan，所以在用Math.floor求出四捨不五入的整數後，要加上|| 0 來處理如果是nan，就為0的狀態

  
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var minimumAverageDifference = function(nums) {
    let sums = [0];
    let result = Infinity;
    let resultIndex = 0;
    for (let i=1;i<nums.length+1;i++) {
        sums[i] = sums[i-1]+nums[i-1];
    }
    for (let i=0;i<nums.length;i++) {
        let currIndex = i+1;
        let averageDiff = Math.floor(sums[currIndex]/currIndex) || 0;
        let averageDiff1 = Math.floor((sums[sums.length-1]-sums[currIndex])/(nums.length-currIndex)) || 0;
        let averageDiff2 = Math.abs(averageDiff - averageDiff1);
        console.log(averageDiff2, i);
        if (averageDiff2 < result) {
            result = averageDiff2;
            resultIndex = i;
        }
    }
    return resultIndex;
};
```

但也有另外的方法 Math.trunc，這個可以直接取其整數，也不用再去判斷nan的狀況，執行上更快

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var minimumAverageDifference = function(nums) {
    let sums = [0];
    let result = Infinity;
    let resultIndex = 0;
    for (let i=1;i<nums.length+1;i++) {
        sums[i] = sums[i-1]+nums[i-1];
    }
    for (let i=0;i<nums.length;i++) {
        let currIndex = i+1;
        let averageDiffLeft = Math.trunc(sums[currIndex]/currIndex);
        let averageDiffRight = Math.trunc((sums[sums.length-1]-sums[currIndex])/(nums.length-currIndex)) || 0;
        let averageDiff = Math.abs(averageDiffLeft - averageDiffRight);
        
        if (averageDiff < result) {
            result = averageDiff;
            resultIndex = i;
        }
    }
    return resultIndex;
};
```

時間複雜度就是O(n)