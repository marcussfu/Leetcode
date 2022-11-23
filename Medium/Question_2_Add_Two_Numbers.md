# Add Two Numbers
難度: Medium (javascript)

題目: 把兩個linked list的值對位相加進位，得到運算結果的list。

Example 1:
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
Example 2:
```
Input: l1 = [0], l2 = [0]
Output: [0]
```
Example 3:
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

限制:
- l1和l2一定有值，不會是空值
- list裡的值都是0-9的範圍
- 不會有0值在最前面的狀態，像是[8,9,0]這種就不會出現。

解法: 
可以分為使用recursion和while兩種

while:
- 先產生一個代表最一開始的節點result，再複製一個crt節點代表目前的節點。
- 另外產生一個carry值表示進位值。
- 每次while迴圈確認l1或l2是不是null或carry是不是大於0，只要有符合就進去迴圈
- 進迴圈後，加總l1和l2和carry
- 計算進位值carry，這邊去比較加總值是不是大於等於10(true=1，false=0)
- 產生一個新節點，讓目前節點crt的next節點等於加總值的餘數，因為carry會到下一次迴圈去加總
- crt目前節點每個迴圈都要等於自已的next，保持目前節點的狀態
- 也判斷l1和l2是不是還有值，沒有則為null，以便下個迴圈可以中止迴圈
- 因為都是讓crt的next來存計算的結果，所以自然最後回傳第一個節點result時，也要回傳result的next，才會是答案。

- 時間複雜度為O(max(m,n)), m和n分別為l1和l2的長度，看那個長度最長，就取最長的那個為時間複雜度
- 空間複雜度為O(max(m,n))，應該與時間複雜度一樣，誰長算誰的。
```
var addTwoNumbers = function(l1, l2) {
    let result = {val: 0, next: null},
        crt = result;
    let carry = 0;

    while(l1 || l2 || carry > 0) {
        let valSum = (l1? l1.val: 0)+(l2? l2.val: 0)+carry;
        carry = valSum >= 10;
        crt.next = {val: valSum%10, next: null};
        crt = crt.next;
        l1 = l1? l1.next:null;
        l2 = l2? l2.next:null;  
    }
    return result.next;
};
```

recursion:
- 用遞迴的方式，是只要l1和l2其一有值，就會繼續遞迴
- l1和l2其一有值的話，就計算l1的val和l2的val和carry(如果有的話)的加總值
- 接著在result產生一節點，記錄目前加總的餘數
- 然後在result的next等於l1的next和l2的next和carry(如果加總值大於等於10)，繼續下個遞迴
- 當然也有一種狀況，是l1和l2都沒值了，但只有進位值carry有值，那就產生一個節點記錄進位值1
- 最後等遞迴到最後一個狀況，就回傳目前的result節點到上一個遞迴，一直回去到最先的result，即為答案。

```
var addTwoNumbers = function(l1, l2) {
    let result = null;
    const carry = arguments[2];
    if (l1 || l2) {
        const val1 = l1? l1.val:0;
        const val2 = l2? l2.val:0;
        const next1 = l1? l1.next: null;
        const next2 = l2? l2.next: null;
        const valSum = val1 + val2 + (carry?carry: 0);
        result = {val: valSum%10, next: null};
        result.next = addTwoNumbers(next1, next2, valSum >= 10);
    }
    else if (carry) {
        result = {val: 1, next: null};
    }
    return result;
}
```