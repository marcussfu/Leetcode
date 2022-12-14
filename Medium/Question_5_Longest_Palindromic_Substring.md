# Longest Palindromic Substring
難度: Medium(javascript&python3)

題目: 給一個字串，回傳這個字串裡最長的回文子字串。

Example 1:
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```
Example 2:
```
Input: s = "cbbd"
Output: "bb"
```
Example 3:
```
Input: s = "abcdedcba"
Output: "abcdedcba"
```

解法: 

- 要用Dynamic Programming
- 對每個項目都往左和往右去尋找，如果是相同的，就繼續往左右找，並比對已經儲存最大的左右index，如果現在的左右index範圍比較大，就置換掉。

- 但這次沒辦法只用一個迴圈，需要用到兩個迴圈O(n^2)
- 第一個迴圈是去遍尋字串的每個項目
- 第二個迴圈，其實不算迴圈，因為數量太小了，不計在時間複雜度上，這個迴圈其實就是針對字串數有奇偶數的狀態，奇數當然不用說，例如字串有五個字，那就是ab'c'ba，中間的那個字是什麼沒關係
- 但如果是偶數，例如: ab'cc'ba，那除了i，i+1的數字也要比對
- 第三個迴圈，就是針對奇偶數字串來兩種方式比較
- 從i左右展開和從i與i+1先比較，再展開這兩種方式
- 只要目前相同回文的左右index範圍比儲存最大的左右回文範圍大，那就取代掉，一直遍尋到最後
- 最後根據最大的左右回文index，回傳s的子字串

javascript
```
function longestPalindrome(s) {
  // ll: Left index of the longest palindrome.
  // rr: Right index of the longest palindrome.
  let ll = 0, rr = 0;

  // Iterate all palindromes with center indices
  // [..., i, ...] or [... i, i+1, ...]
  for (let i = 0; i < s.length; i++) {
    for (let j of [i, i+1]) {
      for (l = i, r = j; s[l] && s[l] === s[r]; l--, r++)

        // Found a new palindrome [l, ..., i, j, ..., r]
        // Update the ll, rr if the newly found palindrome is longer than the
        // existing one.
        [ll, rr] = (r-l+1) > (rr-ll+1) ? [l, r] : [ll, rr];
    }
  }
  return s.substring(ll, rr+1);
}
```

python3的解法，是用two pointer的方式，時間複雜度為O(n^2)，空間複雜度為O(1)。

這方法其實是看別人的解法的，是想要理解為何別人是這樣寫的。

解法其實跟javascript的解法思路差不多，都是遍尋s的每個項目
- 先比較右邊項目有沒有跟目前項目相同的，有的話就表示這是回文，讓right++，一直加到不同為止
- 這樣的方式可以兼顧奇偶數的狀況，例如，"cddb"的話，i在1的時候，就會找到2也一樣，於是right就會右移，直到不同為止，
- 接著比較目前項目的left項目，有沒有跟目前的right項目相同，有的話就表示這是回文，left和right就分別左移右移
- 每次比對完，就要確認目前的回文長度有沒有大於儲存的最大回文長度，有的話就更新
- 這邊想寫些該注意的東西

```
palindrome_len = right - left - 1
if palindrome_len > longest_palindrome_len:
    longest_palindrome_len = palindrome_len
    longest_palindrome_start = left + 1
```

為什麼palindrome_len要減1，為什麼longest_palindrome_start是left + 1

我覺得是因為left和right，在上面的判斷裡，left每次都至少會-1一次，right也是至少會+1一次

這樣做是為了判斷左右的值，但要取出確切範圍的時候，就需要扣回來，所以palindrome_len其實表示right要-1的意思，而longest_palindrome_start就表示left要加1。

python3
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        longest_palindrome_start, longest_palindrome_len = 0, 1

        for i in range(0, n):
            right = i
            while right < n and s[i] == s[right]:
                right += 1
            # s[i, right - 1] inclusive are equal characters e.g. "aaa"
            
            # while s[left] == s[right], s[left, right] inclusive is palindrome e.g. "baaab"
            left = i - 1
            while left >= 0 and right < n and s[left] == s[right]:
                left -= 1
                right += 1
            
            # s[left + 1, right - 1] inclusive is palindromic
            palindrome_len = right - left - 1
            if palindrome_len > longest_palindrome_len:
                longest_palindrome_len = palindrome_len
                longest_palindrome_start = left + 1
            
        return s[longest_palindrome_start: longest_palindrome_start + longest_palindrome_len]
```