# Two Sum
難度: Easy

題目: 給一個 integer陣列和一個 integer target，陣列中任兩個值相加等於target，回傳這兩個值的index。

注意: 同一個項目不能重覆加

(javascript)

Example:
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

Brute force solutions:
```
var twoSum = function(nums, target) {
    for (let i=0;i<nums.length-1;i++) {
        for (let j=i+1;j<nums.length;j++) {
            if (nums[i]+nums[j] == target) {
                return [i,j];
            }
        }
    }
}
```
暴力解的時間複雜度為O(n*n)，因為是兩個迴圈了，速度太慢。

改善為只用一個迴圈來搜尋與用Hash Table來儲存已經找過但目前沒配對上的。

因為在每次迴圈都會拿目前index的值去比對Hash Table裡有沒有符合的，有的話就可以回傳結果了。


```
var twoSum = function(nums, target) {
    const seen = {};
    for (let i=0;i<nums.length;i++) {
        const another = target - nums[i];
        if (another in seen) {
            return [seen[another], i];
        }
        seen[nums[i]] = i;
    }
    return null;
}
```
這樣的時間複雜度為O(n)，而"if (another in seen) {"這邊的時間複雜度為O(1)