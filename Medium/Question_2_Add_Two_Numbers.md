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
- 另外產生一個carry值表示進位值
- 用while迴圈開始檢查l1和l2目前的節點是不是有值，有的話就讓crt把目前的val累加起來，並讓目前的list節點等於next節點以便下一個迴圈檢查。
- 每次迴圈也需要檢查進位值carry是不是有值(1)，如果有的話，也累加到crt的val
- 每次迴圈都要運算目前crt累加的val的進位值的除數(carry)和把crt的val拿去運算餘數，這樣的運算是為了計算是不是已經有進位的狀況。
- 再來就是檢查目前的l1和l2和carry是不是有值，因為有值的話，才需要再產生一個節點來存這個值，不然就可以直接break了。
- 最後就回傳一開始產生的開頭節點result，即為答案。

```
var addTwoNumbers = function(l1, l2) {
    let result = {val: 0, next: null},
        crt = result;
    let carry = 0;

    while(true) {
        if (l1) {
            crt.val += l1.val;
            l1 = l1.next;
        }
        if (l2) {
            crt.val += l2.val;
            l2 = l2.next;
        }
        if (carry > 0) {
            crt.val += carry;
        }

        carry = Math.floor(crt.val/10);
        crt.val = crt.val%10;
        
        if (l1 || l2 || carry > 0) {
            crt.next = {val: 0, next: null};
            crt = crt.next;
        }
        else {
            break;
        }
    }
    return result;
};
```