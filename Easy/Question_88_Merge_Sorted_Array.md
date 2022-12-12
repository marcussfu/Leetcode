# Merge Sorted Array
難度: Easy (javascript&Python)

題目: 得到兩個升序排序的陣列nums1和nums2，和兩個數字m和n，分別代表nums1和nums2有的項目數。

把nums和nums1合成一個升序陣列，但不要回傳新陣列，而是要直接合到nums1這個陣列裡。

Example 1:
```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```
Example 2:
```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```
Example 3:
```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

限制: 能不能讓時間複雜度在O(m+n)

解法: 

很直接想到的方法，就是直接先把nums2的陣列合到nums1後，再排序

- 用兩個index，index1表示nums1目前的應該開始接陣列值的index(m)
- index2給nums2加到nums1用，從0開始
- while迴圈判斷目前index1有沒有超過應該加總的數字(m+n)
- 沒有的話，就是照著目前應該處理的index1，把nums2的值從index2(0)開始加到nums1
- 當然處理完這輪的index1，index1和index2都加1，繼續下一輪的處理，直到index1超過m+n
- 最後再把合成一個陣列的nums1用sort方法排序即可

python3
```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        index1 = m
        index2 = 0
        while index1 < m+n:
            nums1[index1] = nums2[index2]
            index1 += 1
            index2 += 1
        nums1.sort() 
```

javascript
```
var merge = function(nums1, m, nums2, n) {
    let i=m;
    let j=0;
    while(i < m+n) {
        nums1[i] = nums2[j];
        i++;
        j++;
    }
    nums1.sort((x,y) => x-y);
};
```

但其實應該邊把nums2合到nums1的過程中，就應該邊排序完成

- 既然要一邊比較大小排序，一邊合陣列，那two pointer就分別從兩個陣列的最後一個值(各自的最大值)開始比較排序，然後合成到目前最大的值應該放到的位置(m+n-1)
- 接著隨while迴圈，就開始依序往回到排序到0
- 這邊要記得，"idx3--"會先賦值後再減


javascript
```
var merge = function(nums1, m, nums2, n) {
    let idx1 = m - 1, idx2 = n - 1, idx3 = m + n - 1;
   
    while(idx2 >= 0) {
        if(nums1[idx1] > nums2[idx2]) {
            nums1[idx3--] = nums1[idx1--]
        } else {
            nums1[idx3--] = nums2[idx2--];
        }
    }
};
```

python3
```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        p, p1, p2 = m + n - 1, m - 1, n - 1
        while p2 >=0:
            if p2 < 0 or p1 >= 0 and nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1
```

