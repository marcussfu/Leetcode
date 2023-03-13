# Remove Duplicates from Sorted Array
難度: Easy (javascript)

題目: 給一個升序排序且有重覆項目的陣列，回傳把重覆項目拿掉的陣列和不重覆的項目數。

Example 1:
```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
```

Example 2:
```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
```

限制:

不要另外產生別的陣列暫存，只用傳入的陣列移動位置。

解法:

這邊參考了一個覺得很好的方法，時間複雜度也只有O(n)
- last為目前的不重覆值
- count為目前不重覆的數量
- 在for迴圈遍尋陣列中的每個項目時，去比較每個項目是否跟last(不重覆值)一樣
- 不一樣時，便存下不一樣的值到last
- 接著將目前的不重覆值放到nums陣列，根據count(目前不重覆的數量)的位置存放
- "nums[count++] = nums[i]" 假設count目前為0，會先存到nums的0的位置，再++
- 這樣的做法就是為了取得目前不重覆的數量，和把不重覆的值填入目前不重覆的數量index

javascript
```
var removeDuplicates = function(nums) {
    let last = null;
    let count = 0;
    for (let i = 0;i<nums.length;i++) {
        if (nums[i] !== last) {
            last = nums[i];
            nums[count++] = nums[i];
        }
    }
    return count;
}
```

python
```
def removeDuplicates(self, nums: [Int]) -> Int:
    last, count = None, 0
    for (i in range(len(nums))):
        if (nums[i] !== last):
            last = nums[i]
            nums[count] = last
            count += 1
    return count
```