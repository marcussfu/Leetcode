# Search Insert Position
難度: Easy (javascript&python3)

題目: 給一個從小到大排序的數字陣列和target數字，回傳target在這個陣列的index，如果數字陣列裡沒有target，就回傳target應該在數字陣列的哪一個index。

Example 1:
```
Input: nums = [1,3,5,6], target = 5
Output: 2
```
Example 2:
```
Input: nums = [1,3,5,6], target = 2
Output: 1
```
Example 3:
```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

限制: 
- 時間複雜度在 O(log n)
- 1 <= nums.length <= 104
- -104 <= nums[i] <= 104
- -104 <= target <= 104

解法:

暴力解的想法，就是用for迴圈遍尋nums，只要找到target，就回傳index，小於target，就存下index後繼續迴圈，直到超過就回傳記下的index，但這樣的時間複雜度是O(n)，比O(log n)慢

要在有排序的nums中找到target，可以用二進位搜尋法，這個方法時間複雜度就是O(log n)

二進位的方式就是一直找中間值，然後比較中間值是否大於等於或小於目標值，調整start和end的搜尋範圍，然後再一次取中間值比較，一直比到找到，或start比end大，這時候就表示nums裡沒有target，回傳start。

二進位搜尋法可以是遞迴，也可以用while呈現

javascript(recursion)
```
var searchInsert = function(nums, target) {
    return binarySearch(nums, target, 0, nums.length-1);
}

let binarySearch = (arr, x, start, end) => {
    if (start > end) return start;
    
    let mid = Math.trunc((start+end)/2);
    if (arr[mid] == x)
        return mid;
    else if (arr[mid] > x)
        return binarySearch(arr, x, start, mid-1);
    else
        return binarySearch(arr, x, mid+1, end);
};
```

javascript(iterative)
```
var searchInsert = function(nums, target) {
    let start = 0, end = nums.length-1, mid = 0;
    while (start <= end) {
        mid = Math.trunc((start+end)/2);
        if (nums[mid] == target)
            return mid;
        else if (nums[mid] > target)
            end = mid-1;
        else
            start = mid+1;
    }
    return start;
}
```

python3(recursion)
```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        return self.binarySearch(nums, target, 0, len(nums)-1)

    def binarySearch(self, arr, x, start, end):
        if start > end:
            return start

        mid = (start+end)//2
        
        if arr[mid] == x:
            return mid
        elif arr[mid] > x:
            return self.binarySearch(arr, x, start, mid-1)
        else:
            return self.binarySearch(arr, x, mid+1, end)
```

python3(iterative)
```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        start, end = 0, len(nums)-1

        while start <= end:
            mid = (start+end)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                end = mid-1
            else:
                start = mid+1
        return start
```

