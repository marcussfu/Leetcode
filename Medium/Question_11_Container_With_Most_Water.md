# Container With Most Water
難度: Medium(javascript&python3)

題目: 給一個integer的array為height，長度為n，想像為一個水槽，這陣列裡的數字就是水槽的高度，最多的水槽容量為何?

Example 1:
```
8     |              |
7     |--------------|-----|
6     |  |           |     |
5     |  |     |     |     |
4     |  |     |  |  |     |
3     |  |     |  |  |  |  |
2     |  |  |  |  |  |  |  |
1  |  |  |  |  |  |  |  |  |
0  |  |  |  |  |  |  |  |  |

Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

Example 2:
```
Input: height = [1,1]
Output: 1
```

解法: 應該把題目看懂，要想像每個值都是水槽的壁高，要找出那個壁高可以裝最多水，而水是不能傾斜的，所以這樣看的話，像是如果一邊的壁高為1，就表示最多只能裝一格的水高，容量就看寬，即為index與index之間有多寬。

這邊舉例如果一邊為1(index為0)，而另一邊為7(index為8)，那就表示只能裝一格高的水，但寬為(8-0)的8，1乘8就為8，這就表示這個組合只能裝8容量的水。

這邊用two pointer的方式來處理
- 一個index為left，一個index為right，分別從最左和最右開始
- while迴圈去判斷只要left還是小於right，就繼續執行
- 在每次迴圈都去比較目前height[left]和height[right]那個比較小，因為水只能存到比較小的壁高
- 然後把這個比較小的壁高值再乘於寬(right-left)，就是目前left和right所以容納最多的水量
- 再來和目前的result比較，留下最大的水容量值
- 最後是比較left和right哪個值比較小，left比較小，就往前換一個值，看看會不會比較高，right也是一樣，往後換一個值
- 直到left不再小於right，就結束while迴圈了，既回傳目前最大的水容量值

時間複雜度為O(n)，因為我們還是遍尋了每個index。

javascript
```
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
    let left = 0, right = height.length-1, result = 0;
    while (left < right) {
        result = Math.max(result, Math.min(height[left], height[right]) * (right-left));
        if (height[left] < height[right])
            left++;
        else
            right--;
    }
    return result;
};
```

python3
```
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left,right,result = 0, len(height)-1, 0
        while left < right:
            result = max(result, min(height[left], height[right]) * (right-left))
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        return result
```


